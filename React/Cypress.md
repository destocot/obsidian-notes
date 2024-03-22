> uses `mocha`, `chai`, and `jQuery` under the hood 

- pulling typescript definitions
```js
/// <reference types="cypress" />
```

1. [[#Testing Basics]]
2. [[#Aliases]]
3. [[#Complex Inputs]]


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
- we can create an alias for pretty much anything using the `.as()` method.
```js
beforeEach(() => {
	cy.get('[data-test="items-unpacked"]').as('unpackedItems');
	cy.get('[data-test="items-packed"]').as('packedItems');
})
```

```js
cy.get('@unpackedItems').find('label').first().as('firstItem');

cy.get('@firstItem').invoke('text').as('text');
cy.get('@firstItem').find('input[type="checkbox"]').click();

cy.get('@text').then((text) => {
	cy.get('@packedItems').find('label').first().should('include.text', text);
})
```

```js
cy.get('@unpackedItems').find('label').first().as('itemLabel');
cy.get('@itemLabel')
	.invoke('text')
	.then((text) => {
		cy.get('@itemLabel').click();
		cy.get('@packedItems').contains(text);
	});
```

# Complex Inputs

```js
cy.get('#minimum-rating-visibility').as('rating-filter');
cy.get('#restaurant-visibility-filter').as('restaurant-filter');
```

- range input
```js
cy.get('@rating-filter').invoke('val', '7').trigger('input');
cy.get('@rating-filter').should('have.value', '7');
```

- checkbox input
```js
 cy.get('input[type="checkbox"]').check().should('be.checked');
```

- select input
```js
cy.get('@restaurant-filter').select('Taco Bell');
cy.get('@restaurant-filter').should('have.value', 'Taco Bell');
```










# ...

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

#### test href property
```ts
describe("Support button", () => {
  beforeEach(() => {
    cy.viewport(1025, 900);
    cy.visit("http://localhost:3000/dashboard");
  });

  it("should open user's email application with the recipent and subject line filled out", () => {
    cy.get("nav")
      .find("a")
      .contains("Support")
      .should("be.visible")
      .and("have.attr", "href")
      .then(($href) => {
        const href = $href.toString();
        return parseMailto(href);
      })
      .should("deep.equal", {
        recipient: "support@prolog-app.com",
        subject: "Support Request:",
      });
  });
});
```