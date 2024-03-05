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