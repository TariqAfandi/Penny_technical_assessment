# Implementation Notes

## 1. What I changed

Task 1: Fixed two bugs that misclassified a change line.
    The first bug was resolved by comparing values of both price and quanity between baseline and proposed, if there is a mismatch
    then the change would be detected.

    Second bug was resolved by restricting approval access to viewer, where even the viewer had access to approval,
    it checks whether a user has approval policy, if it does not, then it does not allow the action to be triggered by the user.

Task 2: Implemented status filter when selecting the option from it, it would then render rows based on status option selected.

Task 3: Added totals and delta column in diff/preview panel template
    Flipped the audits where it would show the oldest to latest, but it is still a bug as it does not sort the time

Task 4: Permission gating, actions and validation
    Implemented permission gating where if a user does not have approval policy, can only see the detail and list, but cannot 
    do anything that can affect the data.
    Implemented actions through API
    Implemented reject validation where user cannot reject without a valid reason in form.

## 2. Component & state model

index.html, cr-list.component.html, cr-detail.component.html are the main screens/templates that shows in its basic form.

The templates flows into *.spec.ts files.

cr-list.component.spec.ts and cr-detail.component.spec.ts is what becomes the **middle ground** 
between both fixtures and (cr-list.component.ts & cr-detail.component.ts) where it fetches the template of cr-models, cr-api.service.ts.
fixtures is what fills in the data.

the API service goes to cr-detail.component.ts and cr-list.component.ts

The components react with the API service when needed.

fixtures --> cr-detail.component.spec.ts <-- cr.detail.component.ts <-- cr-api.service.ts
                                                    ^           \
                                                    |            \
                                                viewstate       cr-models

fixtures --> cr-list.component.spec.ts <-- cr.list.component.ts <-- cr-api.service.ts
                                                    ^           \
                                                    |            \
                                                viewstate       cr-models

Viewstate exposes both CrList component and CrDetail component where it remains idle.

.html are templates

## 3. Invariants I keep

|diff/preview changed status|diff.util.ts / that reflects quantity and price being compared with baseline to proposed if it does not match, diff() renders rows to cr-detail|
|Restricted approve to unauthorized user|cr-detail.component.ts / canApprove(), Hides approve button from viewer who does not have approve privileges|
|Status Filter|cr-list.component.ts / visibleRows(), Implemented status filter and renders rows based on status|
|Total & Delta column|cr.detail.component.html / Added Total column and Delta column along with data of calculation to display correct results|
|Display timeline chronologically|cr-detail.component / Reversed timeline audits|
|Kept actions hidden from unauthorized users|CanApprove() and CanReject() in cr-detail.component.ts / by checking whether the user has approval policies|
|Approval action call through API|cr-detail.component.ts / approve(), where approval awaits for the API approval method and enables approve action|
|Rejection action call through API|cr-detail.component.ts / reject(), where rejection awaits for the API approval method and enables reject action|
|Requiring rejection reason|cr-detail.component.ts / rejectControl, Validators from Angular Forms if reason is empty so it does not accept the rejection action|

## 4. Testing strategy

I have used npm test for the first two exercises.
I also have used Developer Tools for errors and warnings that was happening for approval and rejection action.
I have used console.log to debug the issues.
I have used try catch as best as I can as I understand it.
I skipped task 5 which is testing because I did not understand how to implement the testing.

## 5. Assumptions

- Tests left a lot of room for interpretation, so I tried my best to use console logs, try catch, developer tools for errors and warnings.
- I thought of using .reverse identifier for timeline chronologically for the audits at the time, but in the end I figured that it had to be sort instead which I was not familiar with.

## 6. Where I used AI

I personally discourage from using AI as the first method of understanding; for the reason that I would want to exercise my brain first.
However, I only ever used AI after I tried my best to read, write and understand the code and its codebase.

- For Approve button functionality where I could not understand how to call the approve function from API.
- For Reject form validation where I did not know where and how to get form validation from.
- Where I did not know how to filter rows according to statusfilter.
- To check what mistakes I have accidentally implemented in the approve/rejection and corrected it.

## 7. What I'd improve with more time

I'd improve by:
- Hiding the approval button from the viewer just as reject button was hidden; not just restricting access.
- After Approving action functionality, the proposed changes do not show the affected new baseline.
- After Rejecting action functionality, the proposed changes do not show the affected new baseline.
- After action functionalities, the preview table does not reload to show the updated changes.
- After Approved/Rejected the proposed changes, choosing the approved/rejected option from the statusFilter 
    does not show details of what CR approved/rejected.
- After Approval or Rejection, the details of price does not refresh.
- After triggering the action functionalities, the timeline displays incorrect chronologically meaning that it 
    flipped the audits rather than sorting the format correctly by date/time.
- Implementing tests for task 5
- Improving the actions functionalities function for duplicates and error states.

## 8. What I have learned from this assessment

Frankly speaking, this has been a mind opener for me where I lacked the knowledge of where I was weak.
In terms of coming to the real world of programming from Polytechnic where making todo apps and websites 
were considered a big thing. However I was disappointed when it came to the real world. 
In the real world, it is a large codebase instead of making a website from scratch; the larger the codebase, 
the more issues & bugs there are and the more features that are needed to solve a specific issue.
It just means that I need the ability and skill to problem solve narrowly rather than understanding the whole 
codebase widely which takes years to understand.
I have went through every emotion imaginable in this assessment, from extreme curiosity to extreme identity crisis. 
It just shows how much I overestimated myself that I'd finish in two days, instead I took all the time; and from 
the overestimation where my skill level is at. It was a humbling experience.
I do know now where I have to aim towards my software journey by my trial and error strategy.