NRCC MATCHDAY CHAT + MANAGER ACCESS: PHONE-FRIENDLY TEST PLAN
=============================================================

Purpose
-------
Validate the development branch before it replaces the currently working matchday website. This checklist is intentionally written for testing from a phone.

CURRENT STATE
-------------
- Development branch: matchday-access-chat
- Draft PR: https://github.com/Sapphiretosin/nrcc-football-balloting/pull/1
- Live site must remain untouched until the checks below pass.
- Do not publish the new Firestore rules yet.
- Manager accounts have not been created.

PHASE 1: REVIEW FROM YOUR PHONE
-------------------------------
1. Open the draft PR and confirm its base is matchday-oct31-visualiser and its head is matchday-access-chat.
2. Review the changed files: index.html, firestore.rules, ACCESS-CHAT-SETUP.txt.
3. Do not merge the PR or deploy the branch yet.

PHASE 2: TEST IN A SAFE ENVIRONMENT
-----------------------------------
Before testing, create a separate Firebase test project or use the Firebase Emulator Suite. Do not experiment with new rules against the production project.

A. Public account and chat
[ ] Email/Password sign-in is enabled in the test project.
[ ] A new public user can register with a display name, email, and password.
[ ] The new user can sign out and sign back in.
[ ] The user can send a message and see it appear.
[ ] A second signed-in user can see and reply to that message.
[ ] A signed-out visitor cannot read or post public chat messages.

B. APJ manager
[ ] Create a test Authentication user.
[ ] As Super Admin, create managerRoles/{UID} with team="apj", displayName, email, active=true.
[ ] APJ manager can read and post in the APJ room.
[ ] APJ manager is denied access to the PFO room.
[ ] APJ manager cannot edit scores, match status, squads, awards, events, or manager roles.
[ ] Successful approved-manager access appears in the Super Admin audit list.

C. PFO manager
[ ] Create a separate test Authentication user.
[ ] As Super Admin, create managerRoles/{UID} with team="pfo", displayName, email, active=true.
[ ] PFO manager can read and post in the PFO room.
[ ] PFO manager is denied access to the APJ room.
[ ] PFO manager cannot edit scores, match status, squads, awards, events, or manager roles.

D. Super Admin and existing features
[ ] Only the intended Super Admin UID can open the existing match controls.
[ ] Super Admin can access both manager rooms.
[ ] Existing match document reads still work.
[ ] Existing event/timeline reads still work.
[ ] Super Admin can create/update/delete match events as expected.
[ ] Scoreboard, squads, awards, substitutions, match report and existing live display still work.

PHASE 3: DEPLOY SAFELY
----------------------
1. Save a copy of the current live site files and current production Firestore rules.
2. Review test results and resolve all failures.
3. Coordinate the reviewed Firestore rules and site deployment; do not deploy only one side without verifying compatibility.
4. Deploy to a staging/preview site first if available.
5. Repeat the checks against staging.
6. Only then approve/merge the PR and deploy to the live site.
7. Re-test the live scoreboard and keep rollback files available.

AUDIT LIMITATION
----------------
The current browser-side audit records successful access events for approved managers. It is not a tamper-proof security log and does not record failed password attempts. Use Firebase Authentication/Google Cloud Logging or a trusted backend if a complete security audit is required.

STOP CONDITION
--------------
If any private-room isolation test fails, if a manager can change match data, or if existing match features break, do not deploy. Restore the previous rules/site and investigate first.
