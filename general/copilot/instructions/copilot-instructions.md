- Conform your implementation to, in this order:
  1. The conventions most local to the file or folder.
  2. The conventions of the project as a whole.
  3. The contemporary conventions of the platforms, languages, and frameworks being used.
  4. General principles for maintainability (see below).
- Use meaningful variable and function names that clearly indicate their purpose.
- Handle errors gracefully and provide informative error messages.
- Provide test coverage for new features and bug fixes.
- Document the code with comments where it may be complex or ambiguous.
- When adding or changing behaviors, update the README.md as necessary.
- Adhere to these general design principles (where it does not conflict with local or project-wide conventions):
  - DRY, with the Rule of Three
  - SOLID
  - KISS, AHA
  - YAGNI
- Design "front-to-back": Drive design decisions by considering the experiences of the end users, then of internal consumers, then of future maintainers.
- When in doubt, prioritize simplicity and readability over cleverness or optimization.
- Favor an incremental workflow:
  1. Recommend selective preliminary refactors existing code where it would simplify the upcoming implementation.
  2. Recommend test coverage of existing code to be changed.
  3. Implement the feature or fix in manageable increments.
    - Each increment should be a complete unit of work that can be merged, reviewed, and deployed on its own.
    - For potentially large change sets, use in-code feature flags to manage incomplete features, and avoid large PRs and long-lived branches.