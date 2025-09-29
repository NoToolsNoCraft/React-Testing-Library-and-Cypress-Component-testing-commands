## Cypress vs React Testing Library: Component Testing Commands

This cheatsheet shows equivalent commands between **Cypress Component Testing** and **React Testing Library (RTL)**, including the necessary **imports**.  

---

### 🟢 Core Commands

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Render a component** | ```js\nimport { mount } from "cypress/react";\ncy.mount(<Component />)\n``` | ```js\nimport { render, screen } from "@testing-library/react";\nrender(<Component />)\n``` |
| **Query element by text** | ```js\n// built into Cypress\ncy.contains("text")\n``` | ```js\nimport { render, screen } from "@testing-library/react";\nscreen.getByText("text")\n``` |
| **Query element by role** | ```js\nimport "@testing-library/cypress/add-commands";\ncy.findByRole("button", { name: /submit/i })\n``` | ```js\nimport { render, screen } from "@testing-library/react";\nscreen.getByRole("button", { name: /submit/i })\n``` |
| **Query element by test id** | ```js\n// built into Cypress\ncy.get("[data-testid='my-element']")\n``` | ```js\nimport { render, screen } from "@testing-library/react";\nscreen.getByTestId("my-element")\n``` |
| **Assert element exists** | ```js\n// built into Cypress\ncy.get("selector").should("exist")\n``` | ```js\nimport "@testing-library/jest-dom";\nexpect(screen.getByText("text")).toBeInTheDocument()\n``` |
| **Assert element not exists** | ```js\n// built into Cypress\ncy.get("selector").should("not.exist")\n``` | ```js\nimport "@testing-library/jest-dom";\nexpect(screen.queryByText("text")).not.toBeInTheDocument()\n``` |
| **Interact: click** | ```js\n// built into Cypress\ncy.get("button").click()\n``` | ```js\nimport { fireEvent } from "@testing-library/react";\nfireEvent.click(screen.getByRole("button"))\n\n// OR with userEvent\nimport userEvent from "@testing-library/user-event";\nuserEvent.click(screen.getByRole("button"))\n``` |
| **Interact: type text** | ```js\n// built into Cypress\ncy.get("input").type("Hello")\n``` | ```js\nimport { fireEvent } from "@testing-library/react";\nfireEvent.change(screen.getByRole("textbox"), { target: { value: "Hello" }})\n\n// OR with userEvent\nimport userEvent from "@testing-library/user-event";\nuserEvent.type(screen.getByRole("textbox"), "Hello")\n``` |
| **Check element value** | ```js\n// built into Cypress\ncy.get("input").should("have.value", "Hello")\n``` | ```js\nimport "@testing-library/jest-dom";\nexpect(screen.getByRole("textbox")).toHaveValue("Hello")\n``` |
| **Wait for async updates** | ```js\n// built into Cypress (auto-retry)\ncy.contains("Loaded")\n``` | ```js\nimport { render, screen } from "@testing-library/react";\nawait screen.findByText("Loaded")\n``` |
| **Run code before/after test** | ```js\n// Mocha built-in (no extra import)\nbeforeEach(() => {...})\nafterEach(() => {...})\n``` | ```js\n// Jest built-in (no extra import)\nbeforeEach(() => {...})\nafterEach(() => {...})\n``` |

---

### 🔎 Additional Queries

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **By label text** | ```js\ncy.findByLabelText("Email")\n``` | ```js\nscreen.getByLabelText("Email")\n``` |
| **By placeholder text** | ```js\ncy.findByPlaceholderText("Enter name")\n``` | ```js\nscreen.getByPlaceholderText("Enter name")\n``` |
| **By display value** | ```js\ncy.findByDisplayValue("Hello World")\n``` | ```js\nscreen.getByDisplayValue("Hello World")\n``` |

---

### ✅ Additional Assertions

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Check text content** | ```js\ncy.get("h1").should("have.text", "Welcome")\n``` | ```js\nexpect(screen.getByRole("heading")).toHaveTextContent("Welcome")\n``` |
| **Check element attribute** | ```js\ncy.get("img").should("have.attr", "alt", "logo")\n``` | ```js\nexpect(screen.getByRole("img")).toHaveAttribute("alt", "logo")\n``` |
| **Check element class** | ```js\ncy.get("button").should("have.class", "active")\n``` | ```js\nexpect(screen.getByRole("button")).toHaveClass("active")\n``` |
| **Check number of elements** | ```js\ncy.get(".item").should("have.length", 3)\n``` | ```js\nexpect(screen.getAllByRole("listitem")).toHaveLength(3)\n``` |

---

### 🎭 Additional Interactions

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Focus / Blur** | ```js\ncy.get("input").focus().blur()\n``` | ```js\nfireEvent.focus(screen.getByRole("textbox"))\nfireEvent.blur(screen.getByRole("textbox"))\n``` |
| **Hover** | ```js\ncy.get("button").trigger("mouseover")\n``` | ```js\nuserEvent.hover(screen.getByRole("button"))\n``` |
| **Select dropdown option** | ```js\ncy.get("select").select("Option 1")\n``` | ```js\nuserEvent.selectOptions(screen.getByRole("combobox"), "Option 1")\n``` |
| **Checkbox / Radio** | ```js\ncy.get("input[type='checkbox']").check()\ncy.get("input[type='radio']").check()\n``` | ```js\nuserEvent.click(screen.getByRole("checkbox"))\nuserEvent.click(screen.getByRole("radio"))\n``` |

---

### ⏱ Async & Debugging

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Wait for condition** | ```js\ncy.get(".item").should("have.length", 3)\n``` | ```js\nawait waitFor(() => expect(screen.getAllByRole("listitem")).toHaveLength(3))\n``` |
| **Debugging** | ```js\ncy.get("selector").debug()\n``` | ```js\nscreen.debug()\n``` |

---

### Notes
- Cypress automatically **waits/retries**; RTL requires `findBy*` or `waitFor`.  
- `@testing-library/cypress` makes queries (`findByRole`, `findByText`, etc.) identical to RTL.  
- Use `@testing-library/jest-dom` for matchers like `.toBeInTheDocument()`, `.toHaveValue()`, `.toHaveAttribute()`.  
- Prefer `userEvent` in RTL for **realistic user behavior** (typing, clicks, selects).  

---
