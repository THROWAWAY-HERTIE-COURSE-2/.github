# THROWAWAY-HERTIE-COURSE-2

**THROWAWAY-HERTIE-COURSE-2** - managed by the Hertie Data Science Lab.
_This page is auto-generated; edits will be overwritten on the next refresh._

## Cohorts

List of cohort orgs registered to receive releases from this course org. _Auto-discovered from the
`cohort-courses-pages.yml` registry_:

- [THROWAWAY-HERTIE-F2026](https://github.com/THROWAWAY-HERTIE-F2026)

## Repositories

List of all repositories associated with the course org; a centralised registry and historical
record of course-related content. _Add new course-related content here, then push to the relevant
cohort org using the GitHub Actions below_.

| Repo | Visibility | Description |
| --- | --- | --- |
| _(no repos yet)_ | | |

## Available actions for faculty & admin

All actions live in the [`.github` repo's Actions tab](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions)
_(automatically bootstrapped from the central
[dsl-teaching-course-setup repo](https://github.com/hertie-data-science-lab/dsl-teaching-course-setup))_:

### One-time setup actions:
- [**Bootstrap cohort**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/bootstrap-cohort.yml) - configure a freshly-created cohort org (sets up scaffold repos), register it with the course org, refresh dropdowns.
- [**Enroll student**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/enroll-student.yml) - grant a student access to the cohort org; provision them with student-level permissions.
- [**New materials repo**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/new-materials.yml) - scaffold a correctly-structured `course-materials-<year>` repo (week folders + the Release buttons).
- [**New assignment**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/new-assignment.yml) - scaffold an `assignment-N-<year>` template repo (starter + autograder on `main`, an empty `solution` branch).
- [**Refresh actions**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/refresh-actions.yml) - repopulate the cohort/week/assignment dropdowns, re-equip content repos, and rebuild this index.

### Weekly cadence actions:
- [**Release materials**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/release-materials.yml) - publish a given week's `lectures/`+`readings/` into a cohort repo.
- [**Release assignment**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/release-assignment.yml) - generate one private repo per student from a chosen `assignment-*` template repo.

NB: alternatively each materials repo *also* carries its own **Release** buttons (run from inside the
repo; there the `week` is a dropdown of that repo's weeks).

- _[**Sync site**](https://github.com/THROWAWAY-HERTIE-COURSE-2/.github/actions/workflows/sync-site.yml) - regenerate a cohort's website from the org structure (releases do this automatically; standard workflow has no need for manual sync)._

## How the actions behave

**Release materials** - run it from the materials repo (per-repo `week` dropdown) or from
the course org's central `.github` repo (pick the source repo, type the week). It copies the *whole*
`lectures/week-N/` and `readings/week-N/` folders - **every file** (any number of lectures
or readings per week) - into the cohort's `materials` repo (private + `students` read),
nested under `week-N/`. Only the weeks you release appear. `include_syllabus` /
`include_readme` (default off) also copy those root files to the cohort root, overwriting.

**Release assignment** - two stages: (1) it freezes a cohort-level template repo
`<assignment>` from your `assignment-*-<year>` template; (2) it generates one private
`<assignment>-<handle>` repo per onboarded student **from that cohort template**, adding
each as collaborator. After the assignment deadline, rerun with **include_solution** to push the
template's `solution` branch into every student repo. Solutions stay on the `solution`
branch so a normal release never leaks them.

**The cohort website** - every cohort has an auto-deployed site `<org>.github.io`. It is regenerated
on every release (and via **Sync site**).

## Repository structure (required)

```
THROWAWAY-HERTIE-COURSE-2/                            <- this COURSE org (persistent)
|-- .github/                      profile + faculty action buttons + cohort registry
|-- course-materials-<year>/      lectures/week-N/   readings/week-N/   (+ syllabus, README)
`-- assignment-<n>-<year>/        is_template repo:
                                    main      -> starter + autograder   (students get this)
                                    solution  -> solution/   (pushed to students on demand)

<Course>-f<year>/                 <- one COHORT org per year (Bootstrap cohort sets it up)
|-- welcome/                      Join issue -> onboard (enrol)
|-- classroom-config/             students.csv  (private roster)
|-- materials/                    released lectures/readings  (students-team read)
|-- <org>.github.io/              auto-deployed website (synced from this structure)
`-- <assignment>-<handle>/        one private repo per student
```

This whole structure is bootstrapped from the central
[`dsl-teaching-course-setup`](https://github.com/hertie-data-science-lab/dsl-teaching-course-setup)
repo (via its **Bootstrap Course Org** action), and the actions above run that same central code.

The course-level actions assume this layout - use **New materials repo** / **New assignment** above to scaffold correctly.

**Materials repo** (`course-materials-<year>`) - the source for Release materials:
- `lectures/week-N/` - one folder per week's lecture files;
- `readings/week-N/` - one folder per week's readings;
- `*syllabus*`, `README.md` at the **root** (optional) - released via the syllabus / README toggles.

**Assignment repo** (`assignment-N-<year>`, an `is_template` repo) - the source for Release assignment:
- **`main` branch** - the starter code + `.github/workflows/autograde.yml`. This is exactly what students receive (native template-generate copies `main` only).
- **`solution` branch** - a `solution/` folder with the model solution. **Solutions MUST live on this branch, never on `main`** - that is what guarantees they are never copied into student repos on generate. They reach students only when you run Release assignment with **include_solution** ticked, which pushes the `solution/` folder into each student repo as a separate, later commit.

---
Maintained by the [Hertie Data Science Lab](https://github.com/hertie-data-science-lab).
