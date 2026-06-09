# Security

create-course is a course-builder template repo. It does not need API keys or
secrets to run.

## Before publishing a generated course

Check the generated course folder before pushing it to a public repo:

- Do not commit `.env` files, API keys, tokens, credentials, private notes, or
  customer data.
- Review `SOURCE.md`, `user.json`, `progress.json`, `PROGRESS.md`, and
  `materials/` before publishing. These files can contain source material, learner
  profile details, progress notes, weak areas, or private examples.
- Brownfield courses copy the original course into a reconstruction workspace.
  Make sure that copied source does not contain private material before publishing.
- NotebookLM support is optional and uses an unofficial third-party tool when
  enabled. Do not upload confidential material unless you understand that tool's
  privacy and account behavior.

## Reporting a security issue

Please do not open a public issue with secrets or exploit details.

Use GitHub private vulnerability reporting if it is enabled for this repo. If it is
not enabled yet, open a minimal public issue saying there is a security concern and
leave out sensitive details.
