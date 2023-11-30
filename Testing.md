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

## Jest

- Matches
  - `test('two plus two is four', () => {expect(2 + 2).toBe(4);});`
  - `.toBe()`, `.toEqual()`, `.not()`
  - `.toBeNull`, `.toBeUndefined`, `.toBeDefined`, `.toBeTruthy`, `.toBeFalsy`
  - `.toBe{Greater,Lesser}{Than,ThanOrEqual}()`, `.toBeCloseTo()`
  - `.toMatch(/regexp/)`, `.toContain()`, `toThrow(/regexp/)`
- Scaffolding 
  - `beforeEach(() => {someSetup();});` (multiple `beforeEach`s allowed)
  - `afterEach(() => {someTearDown();});`
  - Also `beforeAll`, `afterAll`
- Grouping
  - `describe('group1', ()=>{beforeEach(); test(); test();})`
- Mocks -- `const mockCallBack = jest.fn(someLambda)`;
  - `expect(mockCallback.mock.calls).toHaveLength(2);` -- Called twice
  - `expect(mockCallback.mock.calls[1][0]).toBe(17);` -- The first argument of the second call was 17
  - `expect(mockCallback.mock.results[0].value).toBe(42);` -- First call returned 42
- Helpers
  - `@shelf/jest-mongodb`

