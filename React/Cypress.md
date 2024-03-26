> uses `mocha`, `chai`, and `jQuery` under the hood 

- pulling typescript definitions
```js
/// <reference types="cypress" />
```

1. [[#Testing Basics]]
2. [[#Aliases]]
3. [[#Complex Inputs]]
4. [[#Generating Tests]]
5. [[#Checking the Current Path]]
6. [[#Form Validation]]
7. [[#Tasks & Commands]]
8. [[#Network Requests & Sessions]]
9. [[#Mocking & Continuous Integration]]


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

# Generating Tests
- cypress utilizes JavaScript, so we can use JavaScript to populate more tests for our application
```javascript
for (const r of restaurants) {
    it.only(`should only show rows that match ${r} when selected`, () => {
      cy.get('#restaurant-visibility-filter').select(r);
      cy.get('td[headers="whereToOrder-column"]')
        .should('contain', r)
        .and('have.length.at.least', 1);
    });
  }```

```javascript
describe.only('Rating Filter', () => {
    beforeEach(() => {
      cy.get('#minimum-rating-visibility').as('rating');
    });

    for (const r of ratings) {
      it(`should only display items with a rating of ${r} or higher`, () => {
        cy.get('@rating').invoke('val', r).trigger('change');
        cy.get('td.popularity').each(($el) => {
          expect(+$el.text()).to.be.gte(r);
        });
      });
    }
  });
```

# Checking the Current Path
- `cy.location` takes in a parameter for any property on the `window.location` object
	- e.g. `hash, host, hostname, href, origin, pathname, port, protocol, search, toString`
```javascript
it('should navigate to "/sign-in" when you click the "Sign In" button', () => {
	cy.title().should('contain', 'Echo Chamber');
	
	cy.get('[data-test="sign-in"]').click();
	cy.location('pathname').should('equal', '/echo-chamber/sign-in');
});
```

# Form Validation
```javascript
it.only('should require an email', () => {
	cy.get('@submit').click();
	cy.get('[data-test="sign-up-email"]:invalid')
	  .invoke('prop', 'validationMessage')
	  .should('contain', 'Please fill out this field.');
	
	cy.get('[data-test="sign-up-email"]:invalid')
	  .invoke('prop', 'validity')
	  .its('valueMissing')
	  .should('be.true');
	  
    cy.get('[data-test="sign-up-email"]').type('not-an-email');
    cy.get('@submit').click();
    cy.get('[data-test="sign-up-email"]:invalid')
      .invoke('prop', 'validationMessage')
      .should('contain', 'Please enter an email address.');

    cy.get('[data-test="sign-up-email"]:invalid')
      .invoke('prop', 'validity')
      .its('typeMismatch')
      .should('be.true');
});
```

```javascript
  it('should require a password when the email is present', () => {
    cy.get('[data-test="sign-up-email"]').type('j.doe@email.com{enter}');
    cy.get('[data-test="sign-up-password"]:invalid')
      .invoke('prop', 'validationMessage')
      .should('contain', 'Please fill out this field.');
  });
```

# Tasks & Commands

#### Tasks
- when we need to do stuff at the Node.js level, we use Cypress tasks
- tasks live in the `/plugins` directory
```ts
// index.ts
const plugins: Cypress.PluginConfig = (on) => {
	on('task', {
		reset() {
			return reset();
		},
		seed() {
			return seed();
		}
	})
};
```

```javascript
describe('Sign In', () => {
  beforeEach(() => {
    cy.task('seed');
    cy.visit('/echo-chamber/sign-in');
  });

  it('should successfully sign in a user with an email and a password', () => {
    // Sign In
    cy.visit('/echo-chamber/sign-in');
    cy.get('[data-test="sign-in-email"]').type(user.email);
    cy.get('[data-test="sign-in-password"]').type(user.password);
    cy.get('[data-test="sign-in-submit"]').click();

    cy.location('pathname').should('contain', '/echo-chamber/posts');
    cy.contains('Signed in as ' + user.email);
  });
});
```

#### Commands
- commands allow you to batch common operations in to easy-to-use workflows
```js
Cypress.Commands.add('signIn', (user) => {
	cy.visit('/echo-chamber/sign-in');
   cy.get('[data-test="sign-up-email"]').type(user.email);
    cy.get('[data-test="sign-up-password"]').type(user.password);
    cy.get('[data-test="sign-up-submit"]').click();
});

beforeEach(() => {
	cy.task('seed');
	cy.signIn(user);
});
```

# Network Requests & Sessions

```js
cy.intercept(
	{
		method: "GET",
		url: "/users/*"
	},
	[{ username: "Jimi", id: 1 }]
) as('getUsers');
```

- You can also use a **fixture**. Fixtures are effectively dummy data that Cypress will use on your behalf.
```js
cy.intercept('GET', '/activities/*', { fixtures: 'activities.json' }); 
```


==example== Pokemon API

```js
/// <reference types="cypress" />

const pokemon = [
  { id: 1, name: 'Bumblesaur' },
  { id: 2, name: 'Charmer' },
  { id: 3, name: 'Turtle' },
];

describe('Pokémon Search', () => {
  beforeEach(() => {
    cy.visit('/pokemon-search');

    cy.get('[data-test="search"]').as('search');
    cy.get('[data-test="search-label"]').as('label');

    cy.intercept('/pokemon-search/api?*').as('api');
  });
```

```js
  it('should call the API when the user types', () => {
    cy.get('@search').type('bulba');
    cy.wait('@api');
  });
```

```js
  it('should update the query parameter', () => {
    cy.get('@search').type('squir');
    cy.wait('@api');
    cy.location('search').should('equal', '?name=squir');
  });
```

```js
  it('should call the API with correct query parameter', () => {
    cy.get('@search').type('char');
    cy.wait('@api').then((interception) => {
      expect(interception.request.url).to.contain('name=char');
    });
  });
```

```js
  it('should pre-populate the search field with the query parameter', () => {
    cy.visit({ url: '/pokemon-search', qs: { name: 'char' } });
    cy.get('@search').should('have.value', 'char');
  });
```

```js
  it('should render the results to the page', () => {
    cy.intercept('/pokemon-search/api?*', { pokemon }).as('stubbed-api');

    cy.get('@search').type('lol');

    cy.wait('@stubbed-api');

    cy.get('[data-test="result"]').should('have.length', 3);
  });
```

```js
  it('should link to the correct pokémon', () => {
    cy.intercept('/pokemon-search/api?*', { pokemon }).as('stubbed-api');

    cy.get('@search').type('lol');
    cy.wait('@stubbed-api');

    cy.get('[data-test="result"] a').each(($el, index) => {
      const { id } = pokemon[index];
      expect($el.attr('href')).to.contain('/pokemon-search/' + id);
    });
  });
```

```js
  it('should persist the query parameter in the link to a pokémon', () => {
    cy.intercept('/pokemon-search/api?*', { pokemon }).as('stubbed-api');

    cy.get('@search').type('lol');
    cy.wait('@stubbed-api');

    cy.get('[data-test="result"] a').each(($el) => {
      expect($el.attr('href')).to.contain('name=lol');
    });
  });
```

```js
  it('should bring you to the route for the correct pokémon', () => {
    cy.intercept('/pokemon-search/api?*', { pokemon }).as('stubbed-api');
    cy.intercept('/pokemon-search/api/1', { fixture: 'bulbasaur.json' }).as('individual-api');

    cy.get('@search').type('bulba');
    cy.wait('@stubbed-api');

    cy.get('[data-test="result"] a').first().click();
    cy.wait('@individual-api');

    cy.location('pathname').should('contain', '/pokemon-search/1');
  });
```

```js
  it('should immediately fetch pokémon if query parameter is provided', () => {
    cy.intercept('/pokemon-search/api?*', { pokemon }).as('stubbed-api');
    cy.visit({ url: '/pokemon-search', qs: { name: 'bulba' } });

    cy.wait('@stubbed-api').its('response.url').should('contain', '?name=bulba');
  });
});

```

==example== Dog Facts API
```js
/// <reference types="cypress" />

describe('Dog Facts', () => {
  beforeEach(() => {
    cy.visit('/dog-facts');

    cy.get('[data-test="fetch-button"]').as('fetchButton');
    cy.get('[data-test="clear-button"]').as('clearButton');
    cy.get('[data-test="amount-select"]').as('amountSelect');
    cy.get('[data-test="empty-state"]').as('emptyState');

    cy.intercept('/dog-facts/api?*').as('api');
  });
```

```js
  it('should start out with an empty state', () => {
    cy.get('@emptyState');
  });
```

```js
  it('should make a request when the button is called', () => {
    cy.get('@fetchButton').click();
    cy.wait('@api');
  });
```

```js
  it('should no longer have an empty state after a fetch', () => {
    cy.get('@fetchButton').click();
    cy.get('@emptyState').should('not.exist');
  });
```

```js
  it('should adjust the amount when the select is changed', () => {
    cy.get('@amountSelect').select('4');
    cy.get('@fetchButton').click();
    cy.wait('@api').then((interception) => {
      expect(interception.request.url).to.match(/\?amount=4$/);
    });
  });
```

```js
  it('should show the correct number of facts on the page', () => {
    cy.get('@amountSelect').select('6');
    cy.get('@fetchButton').click();
    cy.get('[data-test="dog-fact"]').should('have.length', 6);
  });
```
```js
  it('should clear the facts when the "Clear" button is pressed', () => {
    cy.get('@amountSelect').select('6');
    cy.get('@fetchButton').click();
    cy.get('@clearButton').click();
    cy.get('@emptyState');
  });
```

```js
  it("should reflect the number of facts we're looking for in the title", () => {
    cy.title().should('equal', '3 Dog Facts');

    cy.get('@amountSelect').select('6');

    cy.title().should('equal', '6 Dog Facts');
  });
});

```

==example== Testing Cookies & Sessions

```js
/// <reference types="cypress" />

import '../support/commands-complete';

const user = {
  email: 'first@example.com',
  password: 'password123',
};

export const decodeToken = (token) => JSON.parse(Buffer.from(token, 'base64').toString('utf-8'));
export const encodeToken = (token) => Buffer.from(JSON.stringify(token)).toString('base64');
```

```js
describe('Signing in with a seeded database', () => {
  beforeEach(() => {
    cy.task('seed');
    cy.visit('/echo-chamber/sign-in');
    cy.signIn(user);
  });
  
  it('should be able to log in', () => {
    cy.location('pathname').should('contain', '/echo-chamber/posts');
  });

  it('should set a cookie', () => {
    cy.getCookie('jwt').then((cookie) => {
      const value = decodeToken(cookie.value);
      expect(value.email).to.equal(user.email);
    });
  });
});
```

```js
describe('Setting the cookie', () => {
  beforeEach(() => {
    cy.task('seed');
    cy.setCookie('jwt', encodeToken({ id: 999, email: 'cypress@example.com' }));
    cy.visit('/echo-chamber/sign-in');
  });

  it('should be able to log in', () => {
    cy.location('pathname').should('contain', '/echo-chamber/posts');
  });

  it('show that user on the page', () => {
    cy.contains('cypress@example.com');
  });
});
```

```js
describe('Setting the cookie with real data', () => {
  beforeEach(() => {
    cy.task('seed');
    cy.request('/echo-chamber/api/users')
      .then((response) => {
        const [user] = response.body.users;
        cy.setCookie('jwt', encodeToken(user)).then(() => user);
      })
      .as('user');
    cy.visit('/echo-chamber/sign-in');
  });

  it('should be able to log in', () => {
    cy.location('pathname').should('contain', '/echo-chamber/posts');
  });

  it('show that user on the page', () => {
    cy.get('@user').then((user) => {
      cy.contains(`Signed in as ${user.email}`);
    });
  });
});

```

# Mocking & Continuous Integration

```js
/// <reference types="cypress" />

import '../support/commands-complete';

export const decodeToken = (token) => JSON.parse(Buffer.from(token, 'base64').toString('utf-8'));
export const encodeToken = (token) => Buffer.from(JSON.stringify(token)).toString('base64');
```

```js
describe('Signing in with a seeded database', () => {
  beforeEach(() => {
    cy.setCookie('jwt', encodeToken({ id: 1, email: 'first@example.com' }));

    cy.intercept('GET', '/echo-chamber/api', { fixture: 'posts' }).as('postsApi');
    cy.intercept('GET', /\/echo-chamber\/api\/\d+/, { fixture: 'post' }).as('postApi');
    cy.intercept('GET', '/echo-chamber/api/users', { fixture: 'users' }).as('usersApi');

    cy.intercept('POST', '/echo-chamber/api', {
      statusCode: 201,
      body: {
        post: {
          id: 401,
          content: 'Recently created post',
          createdAt: '2021-12-17T13:12:18.418Z',
          authorId: 220,
        },
      },
    }).as('createPostApi');

    cy.visit('/echo-chamber/posts');

    cy.getData('post-create-content-input').as('newPostInput');
    cy.getData('post-create-submit').as('newPostSubmit');
    cy.getData('post-preview-list').find('article').as('previews');

    cy.fixture('posts').then(({ posts }) => cy.wrap(posts[0]).as('firstPost'));
  });

  it('should render the posts from the API', () => {
    cy.fixture('posts').then(({ posts }) => {
      cy.get('@previews').should('have.length', posts.length);
    });
  });

  it('should navigate to the URL of the post that you clicked on', () => {
    cy.get('@previews').first().click();
    cy.wait('@postApi').then((interception) => {
      cy.location('pathname').should('contain', `/posts/${interception.response.body.post.id}`);
    });
  });

  it('should send a POST request when submitting the form', () => {
    cy.get('@newPostInput').type('Hello world{enter}');
    cy.wait('@createPostApi').its('request.body').should('contain', 'Hello world');
  });
```

```js
  describe('An individual post', () => {
    beforeEach(() => {
      cy.get('@previews').first().click();
      cy.intercept('PATCH', '/echo-chamber/api/*').as('patchRequest');
      cy.intercept('DELETE', '/echo-chamber/api/*').as('deleteRequest');
    });

    it('should show an edit field when you click on the edit button', () => {
      cy.get('[data-test="post-detail-controls-edit-button"]').click();
      cy.get('[data-test="post-detail-edit-form"]');
    });

    it('should send a PATCH request when you send your edit', () => {
      cy.get('[data-test="post-detail-controls-edit-button"]').click();
      cy.get('[data-test="post-detail-edit-form"]').type(' update');
      cy.get('[data-test="post-detail-edit-submit"]').click();
      cy.wait('@patchRequest');
    });

    it('should send a DELETE request when click on the delete button', () => {
      cy.get('[data-test="post-detail-controls-delete-button"]').click();
      cy.wait('@deleteRequest');
    });
  });
});
```

#### Cypress Studio
- interact with your site to add test commands. right click to add assertions.
```json
//cypress.json
{
  "experimentalStudio": true
}
```

```js
  it('does stuff generated by Cypress Studio', () => {
    /* ==== Generated with Cypress Studio ==== */
    cy.get('#minimum-rating-visibility').click();
    cy.get('#restaurant-visibility-filter').select('KFC');
    cy.get(':nth-child(1) > .whereToOrder > .cell').should('have.text', 'KFC');
    /* ==== End Cypress Studio ==== */
  });
});
```

```js
  /* ==== Test Created with Cypress Studio ==== */
  it('Add stuff', function() {
    /* ==== Generated with Cypress Studio ==== */
    cy.get('[data-test="new-item-input"]').clear();
    cy.get('[data-test="new-item-input"]').type('Stuff');
    cy.get('[data-test="add-item"]').click();
    cy.get('#item-6').check();
    cy.get('[data-test="items-packed"] > ul.s-vF8tIk32PFgu > :nth-child(2) > label.s-vF8tIk32PFgu').should('be.visible');
    /* ==== End Cypress Studio ==== */
  });
});
```

#### Run Headless Mode (Continuous Integration)
```bash
npx cypress run
```




---
# ///

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