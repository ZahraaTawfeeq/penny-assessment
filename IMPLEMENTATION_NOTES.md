
# Implementation Notes

## 1. What I changed

- Fixed the diff calculation to detect changes in quantity, unit price, and description.

- Added validation to require a rejection reason.

- Updated the detail page so only authorized users can approve or reject change requests.

- Fixed the loading, empty, and error state handling in the change request list.

- Fixed the status filter so the list displays the correct results when filtering by status.

## 2. Component & state model

- **CrListComponent:** Gets change requests from the mock API and displays them. It manages the list state and the selected status filter.

- **CrDetailComponent:**  Gets a single change request and displays its details, differences, timeline, and available actions. It also handles the Approve and Reject actions.

- **CrApiService:** Provides the mock API methods for loading, approving, and rejecting change requests.

- The components use their state to control what is displayed in the templates, including loading, error, and loaded states.

## 3. Invariants I keep
| Invariant | How / Where |
|---|---|
| A rejection reason is required before rejecting a change request. | `Validators.required` in `CrDetailComponent`. |
| Audit entries are displayed in chronological order (oldest first). | `timeline` getter in `CrDetailComponent`. |
| Only authorized users can approve or reject change requests with `PENDING_APPROVAL` status. | `canApprove` and `canReject` getters in `CrDetailComponent`. |
| Approve and Reject actions call the mock API. | `approve()` and `reject()` methods in `CrDetailComponent`, using `CrApiService`. |
| Quantity, description, and unit price changes are detected. | `computeDiff()` in `diff.util.ts`. |
| The list shows change requests based on the status selected in the filter. | `statusFilter` and `visibleRows` in `CrListComponent`. |

## 4. Testing strategy

In addition to the provided tests, I added:

- **CrListComponent:** A test to check that filtering by status displays the correct change requests.

- **CrDetailComponent:** Tests to check that:

    - Approve is disabled for unauthorized users.

    - Reject is hidden for unauthorized users.

    - Authorized users can approve pending change requests.

    - A rejection reason is required.

    - Audit entries are displayed in chronological order (oldest first).

- **diff.util:** Tests to check that changes in quantity, description, and unit price are detected.

- Ran the test suite using `npm test` to check that the tests pass.

## 5. Assumptions

- Used the `cr_a_o` policy to control permissions for both Approve and Reject actions.

- Kept the Approve button visible but disabled for unauthorized users.

- Compared quantity and description, in addition to unit price, when checking for line-item changes.

## 6. Where I used AI

- Used AI to understand the assessment requirements and clarify some wording.
- Used AI assistance to troubleshoot errors and review my solutions, including exploring possible alternative approaches.
- Used AI assistance to write and structure tests more efficiently, saving time while making sure I understood and reviewed the tests.

## 7. What I'd improve with more time

- Add more component and DOM tests to cover different scenarios.

- Improve feedback messages after successful approval or rejection.

- Improve the UI layout and readability to make information easier to view and understand.
