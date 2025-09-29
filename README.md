# Cypress vs React Testing Library: Component Testing Commands

This cheatsheet shows equivalent commands between **Cypress Component Testing** and **React Testing Library (RTL)**, including the necessary **imports**.

---

## 🟢 Core Commands

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Render a component** | ```javascript<br>import { mount } from "cypress/react";<br>cy.mount(<Component />)<br>``` | ```javascript<br>import { render, screen } from "@testing-library/react";<br>render(<Component />)<br>``` |
| **Query element by text** | ```javascript<br>// built into Cypress<br>cy.contains("text")<br>``` | ```javascript<br>import { render, screen } from "@testing-library/react";<br>screen.getByText("text")<br>``` |
| **Query element by role** | ```javascript<br>import "@testing-library/cypress/add-commands";<br>cy.findByRole("button", { name: /submit/i })<br>``` | ```javascript<br>import { render, screen } from "@testing-library/react";<br>screen.getByRole("button", { name: /submit/i })<br>``` |
| **Query element by test id** | ```javascript<br>// built into Cypress<br>cy.get("[data-testid='my-element']")<br>``` | ```javascript<br>import { render, screen } from "@testing-library/react";<br>screen.getByTestId("my-element")<br>``` |
| **Assert element exists** | ```javascript<br>// built into Cypress<br>cy.get("selector").should("exist")<br>``` | ```javascript<br>import "@testing-library/jest-dom";<br>expect(screen.getByText("text")).toBeInTheDocument()<br>``` |
| **Assert element not exists** | ```javascript<br>// built into Cypress<br>cy.get("selector").should("not.exist")<br>``` | ```javascript<br>import "@testing-library/jest-dom";<br>expect(screen.queryByText("text")).not.toBeInTheDocument()<br>``` |
| **Interact: click** | ```javascript<br>// built into Cypress<br>cy.get("button").click()<br>``` | ```javascript<br>import { fireEvent } from "@testing-library/react";<br>fireEvent.click(screen.getByRole("button"))<br><br>// OR with userEvent<br>import userEvent from "@testing-library/user-event";<br>userEvent.click(screen.getByRole("button"))<br>``` |
| **Interact: type text** | ```javascript<br>// built into Cypress<br>cy.get("input").type("Hello")<br>``` | ```javascript<br>import { fireEvent } from "@testing-library/react";<br>fireEvent.change(screen.getByRole("textbox"), { target: { value: "Hello" }})<br><br>// OR with userEvent<br>import userEvent from "@testing-library/user-event";<br>userEvent.type(screen.getByRole("textbox"), "Hello")<br>``` |
| **Check element value** | ```javascript<br>// built into Cypress<br>cy.get("input").should("have.value", "Hello")<br>``` | ```javascript<br>import "@testing-library/jest-dom";<br>expect(screen.getByRole("textbox")).toHaveValue("Hello")<br>``` |
| **Wait for async updates** | ```javascript<br>// built into Cypress (auto-retry)<br>cy.contains("Loaded")<br>``` | ```javascript<br>import { render, screen } from "@testing-library/react";<br>await screen.findByText("Loaded")<br>``` |
| **Run code before/after test** | ```javascript<br>// Mocha built-in (no extra import)<br>beforeEach(() => {...})<br>afterEach(() => {...})<br>``` | ```javascript<br>// Jest built-in (no extra import)<br>beforeEach(() => {...})<br>afterEach(() => {...})<br>``` |

---

## 🔎 Additional Queries

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **By label text** | ```javascript<br>cy.findByLabelText("Email")<br>``` | ```javascript<br>screen.getByLabelText("Email")<br>``` |
| **By placeholder text** | ```javascript<br>cy.findByPlaceholderText("Enter name")<br>``` | ```javascript<br>screen.getByPlaceholderText("Enter name")<br>``` |
| **By display value** | ```javascript<br>cy.findByDisplayValue("Hello World")<br>``` | ```javascript<br>screen.getByDisplayValue("Hello World")<br>``` |

---

## ✅ Additional Assertions

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Check text content** | ```javascript<br>cy.get("h1").should("have.text", "Welcome")<br>``` | ```javascript<br>expect(screen.getByRole("heading")).toHaveTextContent("Welcome")<br>``` |
| **Check element attribute** | ```javascript<br>cy.get("img").should("have.attr", "alt", "logo")<br>``` | ```javascript<br>expect(screen.getByRole("img")).toHaveAttribute("alt", "logo")<br>``` |
| **Check element class** | ```javascript<br>cy.get("button").should("have.class", "active")<br>``` | ```javascript<br>expect(screen.getByRole("button")).toHaveClass("active")<br>``` |
| **Check number of elements** | ```javascript<br>cy.get(".item").should("have.length", 3)<br>``` | ```javascript<br>expect(screen.getAllByRole("listitem")).toHaveLength(3)<br>``` |

---

## 🎭 Additional Interactions

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Focus / Blur** | ```javascript<br>cy.get("input").focus().blur()<br>``` | ```javascript<br>fireEvent.focus(screen.getByRole("textbox"))<br>fireEvent.blur(screen.getByRole("textbox"))<br>``` |
| **Hover** | ```javascript<br>cy.get("button").trigger("mouseover")<br>``` | ```javascript<br>userEvent.hover(screen.getByRole("button"))<br>``` |
| **Select dropdown option** | ```javascript<br>cy.get("select").select("Option 1")<br>``` | ```javascript<br>userEvent.selectOptions(screen.getByRole("combobox"), "Option 1")<br>``` |
| **Checkbox / Radio** | ```javascript<br>cy.get("input[type='checkbox']").check()<br>cy.get("input[type='radio']").check()<br>``` | ```javascript<br>userEvent.click(screen.getByRole("checkbox"))<br>userEvent.click(screen.getByRole("radio"))<br>``` |

---

## ⏱ Async & Debugging

| Purpose | Cypress Component Testing | React Testing Library |
|---------|---------------------------|------------------------|
| **Wait for condition** | ```javascript<br>cy.get(".item").should("have.length", 3)<br>``` | ```javascript<br>await waitFor(() => expect(screen.getAllByRole("listitem")).toHaveLength(3))<br>``` |
| **Debugging** | ```javascript<br>cy.get("selector").debug()<br>``` | ```javascript<br>screen.debug()<br>``` |

---

## Notes
- Cypress automatically **waits/retries**; RTL requires `findBy*` or `waitFor`.
- `@testing-library/cypress` makes queries (`findByRole`, `findByText`, etc.) identical to RTL.
- Use `@testing-library/jest-dom` for matchers like `.toBeInTheDocument()`, `.toHaveValue()`, `.toHaveAttribute()`.
- Prefer `userEvent` in RTL for **realistic user behavior** (typing, clicks, selects).

---
