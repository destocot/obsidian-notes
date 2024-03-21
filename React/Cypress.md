> uses `mocha`, `chai`, and `jQuery` under the hood 

- pulling typescript definitions
```js
/// <reference types="cypress" />
```

1. [[#Testing Basics]]
2. [[#Aliases]]


# Testing Basics

> best practices is to use the data-*attribute* of an HTML element

**example**
```html
<input data-test="new-item-input" />
```
#### get
```js
cy.get('form').should('exist');
cy.get('form').should('not.exist');
cy.get('form').should('contain.text');
cy.get('[data-test="new-item-input"]').type('Good attitude');
```

#### contains
```js
cy.contains('Add Item)'
cy.contains('Deoderant').should('not.exist');		
```

#### ==examples==

- click on a button
```js
cy.get('[data-test="add-item"]').click();
```

- check last element in a set of elements
```js
cy.get('[data-test="items-unpacked"] li').last().contains('New Item');
```

- iterate through a set of elements
```js
cy.get('[data-itest="items"] li').each(($item) => {
	expect($item.text()).to.include('Tooth');
});

cy.get('[data-test="items"] li').each(($li) => {
	cy.wrap($li).find('[data-test="remove"]').click();
	cy.wrap($li).should('not.exist');
});
```

# Aliases





---

- article on tests: https://kentcdodds.com/blog/write-tests 

**Example Cypress Tests**
```ts
describe("Sidebar Navigation", () => {
  beforeEach(() => {
    cy.viewport(1025, 900);
    cy.visit("http://localhost:3000/dashboard");
  });

  it("links are working", () => {
    const links = [
      { href: "http://localhost:3000/dashboard/issues", label: "Issues" },
      { href: "http://localhost:3000/dashboard/alerts", label: "Alerts" },
      { href: "http://localhost:3000/dashboard/users", label: "Users" },
      { href: "http://localhost:3000/dashboard/settings", label: "Settings" },
      { href: "http://localhost:3000/dashboard", label: "Projects" },
    ];

    // check that links lead to the right pages
    links.forEach((link) => {
      cy.get("nav").contains(link.label).click();
      cy.url().should("eq", link.href);
    });
  });

  it("is collapsible", () => {
    // collapse navigation
    cy.get("nav").contains("Collapse").click();
    // check that links still exist and are functional
    cy.get("nav").find("a").should("have.length", 5).eq(1).click();
    cy.url().should("eq", "http://localhost:3000/dashboard/issues");

	// chec kthat text is not rendered
    cy.get("nav").contains("Issues").should("not.exist");
  });
});
```

- mock API
```ts
import capitalize from "lodash/capitalize";
import mockProjects from "../../fixtures/integration/projects.json";
import { ProjectLanguage } from "api/projects.types";

describe("Project List", () => {
  beforeEach(() => {
    // setup request mock
    cy.intercept("GET", "https://prolog-api.profy.dev/project", {
      fixture: "integration/projects.json",
    }).as("getProjects");

    cy.viewport(1025, 900);
    cy.visit("http://localhost:3000/dashboard");

    // wait for the projects response
    cy.wait("@getProjects");
  });

  it("renders the projects", () => {
    const languageNames = {
      [ProjectLanguage.react]: "React",
      [ProjectLanguage.node]: "Node.js",
      [ProjectLanguage.python]: "Python",
    };

    // get al project cards
    cy.get("main")
      .find("li")
      .each(($el, index) => {
        const projectLanguage = mockProjects[index].language as ProjectLanguage;
        // only test the first project card
        if (index === 0) {
          cy.wrap($el).contains(mockProjects[index].name);
          cy.wrap($el).contains(languageNames[projectLanguage]);
          cy.wrap($el).contains(mockProjects[index].numIssues);
          cy.wrap($el).contains(mockProjects[index].numEvents24h);
          cy.wrap($el).contains(capitalize(mockProjects[index].status));
          cy.wrap($el)
            .find("a")
            .should("have.attr", "href", "/dashboard/issues");
        }
      });
  });
});

```