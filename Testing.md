# Testing Frameworks

## Cypress

- `npm install cypress --save-dev`
- `cypress/e2e/spec.cy.js`
  ```js
  describe('spec.cy.js', () => {
            it('should visit', () => {
              cy.visit('/')
              cy.contains('anchor').click()
              cy.url().should('include', '/path/to/clicked_link')
              cy.contains('substring')
            })

            it('should pass'), () => {
              expect(true).to.equal(true)
            }
          })
  ```

