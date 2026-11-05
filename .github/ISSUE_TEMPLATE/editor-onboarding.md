---
name: Onboarding checklist for a new editor
about: Onboarding checklist as [guest/full] editor for [name]
---

<!-- For discoverability please use the title: "Onboarding checklist as [guest/full] editor for [name] -->

Details of the new editor

- Details that bring you here:
- GitHub handle: 
- Slack handle:
- Record on Airtable:

Useful links

- [Onboarding a new editor](https://devguide.ropensci.org/softwarereview_editor_management.html#onboarding-a-new-editor) 

<!-- start of hidden block

WARNING: This section is hidden but public. Do not write sensitive information.

The intended use case is to accumulate TODOs for the new editor that you may hide for now and send via email in one go, e.g.:

TODO:

- Turn on 2FA.
- Accept invitations to the `@editors` team and the Airtable software review database.
- Propose a day/time to meet before handling the first submission.

end of hidden block -->

----

Full editors start here

- [ ] Add to ["team.json" file of website](https://github.com/ropensci/roweb3/blob/main/data/team/team.json) with `"editor": true`, and also `"stats": true` for stats editors. That will automatically add new editors to the editorial team shown on the [rOpenSci website](https://ropensci.org/software-review/).

- [ ] Inform rOpenSci community manager so that an introductory blog post can be put together.

- [ ] Update the "editor" field in the Airtable ["reviewers-prod" table](https://airtable.com/app8dssb6a7PG6Vwj/tblIBMYXKsF4DwhEW) (this is used to identify and list current editors within this _Dev Guide_) {to Editor}

Guest editors start here

- [ ] If they haven't already done so as guest editors, request that the new editor turn on [two-factor authentication (2FA) for GitHub](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa).

- [ ] Invite them to the rOpenSci GitHub organization as member, as a member of the [`editors` team](https://github.com/orgs/ropensci/teams/editors), and also [`stats-editors`](https://github.com/orgs/ropensci/teams/stats-editors) team for stats editors.
  This will give them appropriate permissions, and ensure that they appear in [the editors dashboard](https://dashboard.ropensci.org/editors).
  - [ ] Send invitation
  - [ ] Invitation accepted

- [ ] Invite them to the AirTable database of software review (linked in the description of the editors-only channel on Slack).
  Ensure invitation is "Read only".
  - [ ] Send invitation
  - [ ] Invitation accepted

- [ ] Update the "editor" field in the Airtable ["reviewers-prod" table](https://airtable.com/app8dssb6a7PG6Vwj/tblIBMYXKsF4DwhEW) (this is used to identify and list current editors within this _Dev Guide_) {to Guest}

- [ ] Invite them to the private "editors-only" channel in rOpenSci's Slack workspace (and to the Slack workspace in general if they're not yet there).

Meet or chat with at least one [staff member of the software peer-review project](https://ropensci.org/software-review/) acting as an onboarding mentor: 

- [ ] Before handling the first submission for personal connection and questions
- [ ] After handling the first submission for a retrospective and to discuss interest in becoming a full editor
  - What went well?
  - What didn't go so well?
  - How can we improve?

