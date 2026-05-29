# Fact Sheet: Architecting a GitHub Enterprise Cloud Instance for Schools Using the Campus Program

## Purpose

The Campus Program gives schools and educational institutions access to a free, self-served GitHub Enterprise Cloud enterprise plan. A well-designed enterprise architecture helps institutions centralize governance, simplify access management, support teaching and research, and give students, faculty, and staff a consistent developer experience.

GitHub Enterprise Cloud provides an **enterprise account**, which acts as the central place to manage multiple organizations, policies, members, teams, audit activity, and access controls.

This fact sheet is intended for schools, universities, colleges, districts, research institutions, and educational organizations that are adopting the Campus Program and need guidance on how to structure their enterprise instance.

---

## Important Campus Program Limitations

> [!IMPORTANT]
> The Campus Program provides a free, self-served enterprise plan for qualifying educational institutions. However, it does **not** include every feature, commercial entitlement, support option, security product, or compliance resource that may be available with a paid GitHub Enterprise agreement.

Schools should design their enterprise architecture with the following limitations in mind.

| Area | Campus Program limitation | Architectural impact |
|---|---|---|
| **Enterprise Managed Users** | Enterprise Managed Users, or EMU, are **not available** through the Campus Program. | Schools should plan to use personal GitHub.com accounts with SAML-based authentication rather than institution-provisioned managed user accounts. |
| **Data residency** | GitHub Enterprise Cloud with data residency is **not available** through the Campus Program. | Schools should not design the environment around GHE.com regional hosting or data residency controls. |
| **GitHub Copilot Business** | GitHub Copilot Business is **not included** in the Campus Program enterprise plan. | Copilot adoption, licensing, governance, and rollout should be planned separately from the Campus Program enterprise. |
| **Individual Copilot access** | The Campus Program does **not** automatically grant individual Copilot licenses to students, staff, or faculty. | Users must bring their own Copilot access, or the school must separately fund and assign licenses. |
| **GitHub Actions** | The Campus Program does **not** automatically grant expanded paid Actions usage. | Schools should plan Actions usage around included allowances, user-owned entitlements, or separately funded metered billing. |
| **GitHub Codespaces** | The Campus Program does **not** automatically grant paid Codespaces usage. | Schools should decide whether users bring their own access or whether the institution will fund Codespaces with budgets and payment limits. |
| **GitHub Advanced Security / paid security products** | Paid security products, including GitHub Advanced Security capabilities, are **not included** as part of the Campus Program enterprise plan. | Schools should not assume access to paid code security, secret protection, or enterprise-wide Advanced Security licensing unless separately purchased or otherwise provided. |
| **Premium Support** | GitHub Premium Support is **not included**. | Schools should plan for standard available support paths and self-service administration rather than premium support SLAs, incident management, or named support resources. |
| **Security documentation** | Because no NDA is required for the Campus Program, only publicly available security documentation, such as SOC 2 documentation, should be expected. | Schools should plan security reviews around public documentation and the GitHub Trust Center rather than NDA-gated documentation packages. |
| **Add-on metered products** | Metered or licensed products must be purchased, enabled, or funded separately. | Schools can work with GitHub Sales and add a payment instrument, such as a credit card or eligible Azure Subscription ID, to support paid usage. |

GitHub documentation for GitHub Enterprise Cloud may describe features that are available in paid or separately contracted GitHub Enterprise offerings. For Campus Program institutions, the limitations above should be treated as program-specific constraints when designing the enterprise.

---

## Recommended Architecture at a Glance

**Recommended model for most schools:**

```text
GitHub Enterprise Account
│
├── Organization: teaching
│   ├── Repositories for courses, assignments, templates, examples
│   └── Teams for faculty, teaching assistants, students, course cohorts
│
├── Organization: research
│   ├── Repositories for labs, research groups, grant-funded projects
│   └── Teams for PIs, researchers, graduate students, collaborators
│
├── Organization: institutional-it
│   ├── Repositories for internal tooling, infrastructure, automation
│   └── Teams for IT, security, platform administrators
│
├── Organization: open-source-or-community
│   ├── Repositories intended for public or community collaboration
│   └── Maintainer teams for approved public-facing projects
│
└── Optional Organization: restricted-or-sensitive
    ├── Repositories with stricter policy requirements
    └── Limited membership and stronger repository rules
```

Start with a **small number of organizations** and expand only when there is a clear governance, access, or collaboration reason. It is usually easier to add organizations later than to remove or consolidate them.

---

## Key Design Principles

### 1. Use the enterprise account as the governance layer

The enterprise account should be the school’s top-level administrative boundary. Use it to centralize:

- Enterprise owners and delegated administrators
- Authentication and access policies
- Organization creation and ownership
- Audit and compliance oversight
- Repository and security policies
- Billing or usage visibility, where applicable

The enterprise account should be owned and managed by a small number of trusted institutional administrators, typically from central IT, academic technology, research computing, or a similar governance function.

---

### 2. Use organizations for governance boundaries, not every class or team

Organizations should usually represent a major administrative or governance boundary, such as:

- Teaching and learning
- Research
- Institutional IT
- Public/open-source work
- Restricted or sensitive work

Avoid creating a new organization for every course, semester, lab, student group, or department unless they require distinct policies, owners, or access boundaries.

Organizations are best used to group related work or repositories with similar governance, security, or access requirements.

---

### 3. Use teams for people, permissions, and scale

Use teams to represent groups of people, such as:

- `faculty`
- `teaching-assistants`
- `students-2026`
- `course-cs101-spring-2026`
- `research-lab-smith`
- `institutional-it-admins`
- `security-reviewers`

Teams should be the primary way to grant repository access, delegate responsibilities, and manage permissions at scale.

Where possible, align teams with identity provider groups or institutional roles so membership can be managed consistently.

---

## Identity and Access Management

Schools using the Campus Program should plan around **personal GitHub.com accounts with SAML-based authentication**.

> [!WARNING]
> Enterprise Managed Users are **not available** through the Campus Program. Do not design the enterprise architecture around institution-provisioned managed user accounts, EMU namespaces, SCIM-provisioned managed users, or GHE.com data-residency environments.

### Recommended model: Personal accounts with SAML

Users sign in with their personal GitHub.com accounts, and the institution can apply SAML-based access controls.

This model works well for schools because students, faculty, researchers, and staff often need to participate in the broader GitHub.com ecosystem, including:

- Open source communities
- Internships
- Research collaborations
- Personal portfolios
- Student projects
- External academic partnerships

Use this model when:

- Students need GitHub identities they can keep after graduation
- Faculty or researchers collaborate outside the institution
- Open source contribution is a core use case
- The institution wants a lower-overhead identity lifecycle model
- The school is adopting GitHub through the Campus Program

Recommended practices:

- Configure SAML single sign-on where available
- Use teams for authorization and repository access
- Use identity provider groups where supported
- Document onboarding and offboarding processes
- Avoid granting broad organization ownership to students or temporary users
- Review organization membership regularly
- Use least-privilege access for external collaborators and guests

---

## SAML: Enterprise-Level vs Organization-Level

For most schools, **enterprise-level SAML** is the preferred default if using SAML. It provides a consistent authentication experience and applies across organizations in the enterprise.

Use **enterprise-level SAML** when:

- You want one consistent login policy across the institution
- You need to protect internal repositories across organizations
- Central IT owns GitHub identity governance
- Most users authenticate through the same institutional identity provider

Use **organization-level SAML** only when different schools, campuses, departments, or research entities require different identity providers, policies, or rollout timelines.

---

## Repository Visibility Guidance

Use repository visibility intentionally.

| Visibility | Recommended school use |
|---|---|
| **Private** | Course assignments, unpublished research, institutional systems, sensitive work |
| **Internal** | Shared institutional code, templates, reusable workflows, examples, innersource projects |
| **Public** | Open source projects, public research outputs, community-facing resources |

Recommended approach:

- Use **private repositories** for student work, coursework, unpublished research, institutional systems, and sensitive projects.
- Use **internal repositories** for reusable institutional resources that should be discoverable by members of the enterprise.
- Use **public repositories** only when the work is intended for public release, open source collaboration, or external community engagement.

For innersource, use **internal** visibility where appropriate so members across the enterprise can discover and reuse work.

---

## Suggested Organization Patterns

### Teaching organization

Use for instructional material and classroom workflows.

Recommended repositories:

- Course templates
- Assignment starter code
- Example projects
- Shared teaching tools
- Reusable GitHub Actions workflows for courses
- Documentation for classroom GitHub usage
- Demo applications and sample projects

Recommended teams:

- Faculty
- Teaching assistants
- Course-specific student cohorts
- Department-level instructors
- Academic technology staff
- Curriculum developers

Recommended practices:

- Use private repositories for student work
- Use template repositories for starter assignments
- Use teams for each course or semester
- Archive repositories after the course ends
- Avoid giving students broad organization ownership
- Establish naming conventions for courses and semesters
- Document expectations for acceptable use and academic integrity

Example repository naming patterns:

- `cs101-spring-2026-starter`
- `cs101-spring-2026-examples`
- `data-science-lab-template`
- `intro-python-assignment-01`

---

### Research organization

Use for labs, research projects, grants, and collaborations.

Recommended repositories:

- Lab code
- Research software
- Data-processing pipelines
- Manuscript-supporting code
- Grant-funded project repositories
- Shared analysis tools
- Reproducibility packages
- Research documentation sites

Recommended teams:

- Principal investigators
- Lab members
- Graduate students
- Undergraduate researchers
- External collaborators, where supported
- Research software engineers
- Research computing staff

Recommended practices:

- Separate sensitive or regulated research into a dedicated organization when policy requirements differ
- Prefer team-based access over individual repository permissions
- Use internal repositories for reusable research infrastructure
- Use public repositories only after review and approval
- Establish publication and archival guidance for research code
- Clarify ownership and access expectations for departing researchers

Example repository naming patterns:

- `lab-name-analysis-pipeline`
- `grant-project-modeling`
- `manuscript-2026-code`
- `research-software-toolkit`

---

### Institutional IT organization

Use for central IT, DevOps, platform engineering, and institutional automation.

Recommended repositories:

- Infrastructure-as-code
- Automation scripts
- Internal developer tooling
- Shared Actions workflows
- Security or compliance tooling
- Configuration management
- Internal documentation
- Integration scripts

Recommended teams:

- Enterprise administrators
- Organization owners
- Platform engineers
- Security reviewers
- Audit or compliance teams
- Help desk or support engineers
- Identity and access administrators

Recommended practices:

- Prefer organization-owned repositories over user-owned repositories
- Use private repositories for operational tooling
- Use internal repositories for shared infrastructure patterns and reusable workflows
- Limit administrative access to trusted staff
- Require code review for production-impacting changes
- Use branch protection or repository rulesets for critical repositories

Organization-owned repositories are recommended over user-owned repositories because they provide stronger administrative and security features and remain accessible as membership changes.

---

### Open-source or community organization

Use for public-facing projects, community engagement, open educational resources, and approved open source work.

Recommended repositories:

- Open source research software
- Public educational materials
- Community-facing tools
- Documentation sites
- Public datasets or dataset tooling, where appropriate
- Open educational resources

Recommended teams:

- Project maintainers
- Faculty sponsors
- Community moderators
- Communications or public engagement reviewers
- Open source program office contributors, if applicable

Recommended practices:

- Define a public release review process
- Use clear licenses
- Include contribution guidelines
- Include codes of conduct where appropriate
- Review repositories for sensitive data before publication
- Separate public collaboration from internal coursework and sensitive research

---

### Restricted or sensitive organization

Use when repositories require stronger controls than the rest of the institution.

Examples:

- Regulated research
- Security-sensitive infrastructure
- Institutional data systems
- Contractual or grant-restricted work
- Sensitive integrations
- Projects involving confidential institutional data

Recommended practices:

- Keep membership small
- Use stricter base permissions
- Require branch protections or repository rulesets
- Limit repository creation
- Review external collaborator access
- Separate from general teaching and research work
- Document the reason for restricted access
- Review access on a recurring schedule

Group repositories with similar security, policy, or access requirements into the same organization so settings can be applied at scale.

---

## Innersource and Reuse

Schools should encourage internal sharing of code, workflows, and teaching materials where appropriate.

Recommended innersource assets:

- Reusable GitHub Actions workflows
- Shared course templates
- Common research software libraries
- Internal packages
- Documentation sites
- Example repositories
- Starter projects
- Infrastructure modules
- Analysis pipelines

Recommended practices:

- Make repositories discoverable when they do not contain sensitive information
- Document projects with clear `README.md` files
- Use repository topics
- Share reusable Actions workflows and packages across the enterprise
- Create contribution guidelines for widely used internal projects
- Identify maintainers for shared assets
- Use internal visibility for reusable institutional work

Examples of reusable institutional repositories:

- `shared-actions-workflows`
- `course-template-python`
- `research-computing-examples`
- `institutional-style-guide`
- `common-data-tools`

---

## External Collaborators and Guests

Schools often work with:

- Visiting researchers
- Adjunct faculty
- Partner institutions
- Vendors
- Alumni contributors
- Open source maintainers
- Contract developers
- Grant partners

Recommended approach:

- Use the narrowest access that supports the collaboration
- Prefer repository-level access for external collaborators
- Avoid adding external users broadly to organizations unless necessary
- Review external access regularly
- Set clear expiration or review dates for temporary collaborations
- Remove access promptly when collaborations end
- Avoid granting external collaborators administrative privileges unless required

Because Enterprise Managed Users are not available through the Campus Program, schools should design guest and collaborator workflows around personal GitHub.com accounts.

---

## Individual Product Entitlement Limitations

> [!WARNING]
> The Campus Program enterprise plan does **not** automatically confer individual access to GitHub Copilot, GitHub Actions, or GitHub Codespaces.

The Campus Program provides the school with access to a free, self-served enterprise plan, but individual user access to separately metered or licensed products must be planned separately.

This means students, staff, and faculty should not assume they automatically receive:

- GitHub Copilot individual or organization-assigned licenses
- GitHub Actions paid minutes or expanded usage beyond included free allowances
- GitHub Codespaces paid usage
- Other metered product entitlements

Schools have several options for enabling these products.

---

### Option 1: Bring your own account

Users may use their own GitHub accounts and any benefits, subscriptions, or entitlements already associated with those accounts.

This may be appropriate when:

- Students already have individual GitHub benefits
- Faculty or staff already have separate GitHub access
- The school does not want to centrally fund add-on products
- Use is optional or limited to specific individuals
- A course or program does not require centrally managed paid usage

Examples:

- A student uses their own individual Copilot entitlement.
- A faculty member uses a separately purchased GitHub product.
- A researcher uses their own GitHub account and personal billing arrangement.

---

### Option 2: School-managed payment limits and budgets

The school can configure billing controls, budgets, or payment limits to provide access to eligible paid or metered GitHub products for students, staff, or faculty.

This may be appropriate when:

- The school wants to sponsor usage for approved users
- A course, department, or program needs controlled access to Copilot, Actions, or Codespaces
- The institution wants to prevent uncontrolled metered spend
- Usage should be capped by budget, group, or administrative policy
- A pilot program needs limited funding before a broader rollout

Recommended practices:

- Define who is eligible for funded usage
- Set payment limits or budgets before enabling metered products
- Monitor usage regularly
- Assign administrative ownership for billing oversight
- Communicate limits clearly to faculty, staff, and students
- Create a process for requesting increased limits

---

### Option 3: Purchase add-on metered products with GitHub Sales

The school can work with a GitHub Sales team member to purchase or enable add-on metered products by adding a payment instrument.

Supported payment approaches may include:

- Credit card
- Azure Subscription ID

This may be appropriate when:

- The school wants centralized purchasing
- Multiple departments need funded access
- The institution wants to align GitHub billing with cloud procurement
- A program requires predictable administrative oversight
- The school wants to enable usage-based products at enterprise or organization scale

Potential add-on or separately funded products may include:

- GitHub Copilot
- GitHub Actions paid usage
- GitHub Codespaces paid usage
- GitHub Packages paid usage
- Git Large File Storage usage
- GitHub Advanced Security or related paid security products, where available

> [!NOTE]
> Some Azure subscription types may not be supported as GitHub payment methods. Schools should verify payment eligibility before planning an Azure-funded rollout.

---

## Copilot Considerations

> [!WARNING]
> GitHub Copilot Business is **not included** in the Campus Program enterprise plan.

Schools should treat Copilot as a separate adoption, licensing, and governance decision.

Do not assume the Campus Program enterprise includes:

- Copilot Business licensing
- Individual Copilot licenses for students
- Individual Copilot licenses for faculty
- Individual Copilot licenses for staff
- Centralized Copilot policy management
- Copilot usage reporting
- Enterprise Copilot rollout controls
- Copilot procurement or legal review resources

If a school wants to adopt Copilot, it should plan that work separately, including:

- Legal review
- Privacy review
- Security review
- Accessibility review
- Academic integrity policy
- Faculty guidance
- Student usage expectations
- Funding model
- License assignment process
- Support model

Possible Copilot adoption models:

| Model | Description |
|---|---|
| **Bring your own Copilot** | Users rely on their own individual Copilot access or benefits. |
| **Course-funded Copilot** | A school, department, or course funds licenses or usage for a specific cohort. |
| **Institution-funded Copilot** | The institution funds Copilot access for approved students, staff, or faculty. |
| **Pilot program** | A limited group receives access while the school evaluates impact, governance, and cost. |

---

## Actions and Codespaces Considerations

> [!WARNING]
> The Campus Program does **not** automatically provide expanded paid GitHub Actions or GitHub Codespaces usage.

Schools should plan Actions and Codespaces usage separately from the enterprise architecture.

### GitHub Actions

GitHub Actions may be used for:

- Course autograding workflows
- Continuous integration
- Research software testing
- Documentation publishing
- Infrastructure automation
- Reusable teaching workflows

Recommended practices:

- Start with small, controlled usage
- Monitor minutes and storage consumption
- Use budgets or payment limits where available
- Provide faculty with guidance on workflow cost implications
- Avoid unbounded workflows in student repositories
- Use reusable workflows for common course patterns
- Review public repository workflows carefully

### GitHub Codespaces

GitHub Codespaces may be used for:

- Standardized classroom development environments
- Research computing workshops
- Onboarding students to a course
- Reducing local setup issues
- Short-term labs or bootcamps

Recommended practices:

- Define who pays for Codespaces usage before rollout
- Use budgets or payment limits
- Create dev containers that are appropriately sized
- Document when students should stop or delete codespaces
- Avoid assuming unlimited compute availability
- Pilot with a small course or cohort before broader adoption

---

## Data Residency Considerations

> [!WARNING]
> GitHub Enterprise Cloud with data residency is **not available** through the Campus Program.

Schools using the Campus Program should assume their enterprise will use the standard GitHub Enterprise Cloud hosting model and should not design around:

- GHE.com regional hosting
- A dedicated data-residency subdomain
- Selection of a specific data storage region
- Enterprise Managed Users as a prerequisite for data residency

If the institution has strict data residency, jurisdictional storage, or regulated research requirements, those requirements should be evaluated separately before using the Campus Program for that work.

Examples of work that may require additional review:

- Regulated research data
- Human subjects research
- Sensitive institutional data
- Government-restricted work
- Contractually restricted data
- Data subject to local residency requirements

Recommended approach:

- Consult institutional legal, privacy, and security teams
- Classify data before storing it in repositories
- Use private repositories for sensitive work
- Avoid storing regulated datasets directly in GitHub unless approved
- Separate restricted work from general teaching and research repositories
- Document which types of data are permitted in GitHub

---

## Security, Compliance, and Support Limitations

> [!WARNING]
> Paid security products, Premium Support, and NDA-gated security documentation are **not included** with the Campus Program enterprise plan.

### Security product limitations

The Campus Program enterprise plan should not be treated as including paid GitHub Advanced Security products or enterprise-wide paid security entitlements.

Schools should verify separately before assuming access to:

- GitHub Code Security
- GitHub Secret Protection
- GitHub Advanced Security licensing
- Enterprise-wide paid security management features
- Enterprise-wide paid vulnerability management features
- Paid code scanning capabilities
- Paid secret scanning capabilities
- Paid dependency management capabilities

Recommended practices:

- Do not assume paid security products are available by default
- Confirm entitlements before designing security workflows
- Use available repository and organization security settings
- Establish secure coding guidance for courses and research groups
- Use branch protections or repository rulesets for critical work
- Avoid committing secrets, credentials, or sensitive data
- Provide training on secure GitHub usage

---

### Support limitations

GitHub Premium Support is not included with the Campus Program.

Schools should not assume access to:

- Premium Support SLAs
- 24x7 premium ticket handling
- Phone callback support
- Incident management
- Named Customer Reliability Engineers
- Technical advisory hours
- Premium health checks

Recommended practices:

- Build an internal support model
- Identify enterprise and organization administrators
- Create internal documentation for common workflows
- Establish a help path for students, faculty, and staff
- Use standard available GitHub support resources
- Document escalation paths for account, access, and billing issues

---

### Security documentation limitations

Because the Campus Program does not require an NDA, schools should expect to rely on publicly available security and compliance documentation.

For security reviews, schools should plan around:

- Public GitHub security documentation
- Public GitHub compliance resources
- SOC 2 documentation where available
- Public GitHub Trust Center materials

Schools should not assume access to:

- NDA-gated security questionnaires
- Private compliance packages
- Custom security documentation
- Custom contractual security exhibits
- Bespoke vendor risk documentation
- Non-public audit materials

Recommended practices:

- Start security reviews early
- Align review expectations with available public documentation
- Document Campus Program limitations for procurement and compliance teams
- Route specialized requirements through appropriate institutional channels
- Engage GitHub Sales if paid offerings, add-ons, or contractual terms are required

---

## Recommended Policy Defaults

| Area | Recommended default |
|---|---|
| Enterprise owners | Keep very limited; use delegated roles where possible |
| Organization creation | Centralize or require approval |
| Repository ownership | Prefer organization-owned repositories |
| User access | Manage through teams, not individuals |
| Identity model | Use personal GitHub.com accounts with SAML-based authentication |
| Enterprise Managed Users | Do not design around EMU; it is not available through the Campus Program |
| SAML | Prefer enterprise-level SAML for consistency |
| Repository visibility | Private by default for teaching and sensitive work; internal for reusable institutional work |
| Public repositories | Require institutional review or defined approval workflow |
| External collaborators | Grant least-privilege repository access |
| Course repositories | Archive or clean up after each term |
| Documentation | Maintain enterprise and organization `README.md` files |
| Copilot | Plan separately; not included by default |
| Actions | Monitor usage and plan paid usage separately |
| Codespaces | Use budgets or payment limits before broad rollout |
| Data residency | Do not assume availability through the Campus Program |
| Paid security products | Verify separately before assuming access |
| Premium Support | Not included; plan standard support and internal support processes |
| Security documentation | Use public documentation and SOC 2 where available |

---

## Adoption Checklist

### Phase 1: Plan

- Identify primary use cases:
  - Teaching
  - Research
  - Institutional IT
  - Open source
  - Regulated or sensitive work
- Confirm Campus Program limitations with stakeholders
- Choose the identity model:
  - Personal GitHub.com accounts with SAML-based authentication
- Confirm that Enterprise Managed Users are not part of the architecture
- Confirm that data residency is not part of the Campus Program architecture
- Define initial organizations
- Define enterprise owner and organization owner model
- Define repository visibility standards
- Define external collaborator policies
- Define public repository approval process
- Identify whether Copilot, Actions, Codespaces, or other metered products require separate funding
- Identify billing owner or payment instrument owner if add-on products will be used

---

### Phase 2: Configure

- Set up the enterprise account
- Configure authentication
- Create initial organizations
- Create teams aligned to departments, courses, labs, and roles
- Set organization base permissions
- Configure repository policies and rulesets
- Establish repository naming conventions
- Establish team naming conventions
- Create starter documentation
- Configure billing controls, budgets, or payment limits if using add-on metered products
- Define support and escalation contacts

---

### Phase 3: Pilot

- Start with a small number of departments, courses, or labs
- Validate onboarding and offboarding
- Test repository creation and access workflows
- Confirm faculty, student, and researcher experience
- Test external collaborator workflows
- Confirm whether users can bring their own account entitlements
- Test budget or payment limit behavior if using paid usage
- Document support paths
- Gather feedback before scaling

---

### Phase 4: Scale

- Add more departments and programs
- Publish reusable templates and documentation
- Create internal support and governance processes
- Review audit logs and policy effectiveness
- Periodically clean up old repositories, teams, and organizations
- Review external collaborator access
- Review billing and metered usage
- Review public repositories
- Revisit organization structure as adoption grows
- Update documentation as GitHub features and institutional policies change

---

## Example Governance Model

| Role | Recommended responsibility |
|---|---|
| **Enterprise owners** | Manage top-level enterprise settings, identity policies, organization creation, and governance. |
| **Organization owners** | Manage organization-level settings, repositories, teams, and permissions. |
| **Faculty maintainers** | Manage course repositories, assignments, teaching materials, and student access. |
| **Research leads / PIs** | Manage lab or project repositories, collaborators, and research access. |
| **IT administrators** | Manage institutional automation, infrastructure repositories, and support processes. |
| **Security reviewers** | Review sensitive repositories, public release requests, and policy compliance. |
| **Billing administrators** | Manage payment instruments, budgets, payment limits, and metered product usage. |

---

## Suggested Naming Conventions

### Organizations

Use short, clear names that reflect governance boundaries.

Examples:

- `school-teaching`
- `school-research`
- `school-it`
- `school-open-source`
- `school-restricted`

### Teams

Use role-based and purpose-based team names.

Examples:

- `faculty`
- `teaching-assistants`
- `students-2026`
- `course-cs101-spring-2026`
- `research-lab-smith`
- `security-reviewers`
- `platform-admins`

### Repositories

Use consistent naming patterns that make ownership and purpose clear.

Examples:

- `cs101-spring-2026-assignment-01`
- `cs101-spring-2026-starter`
- `research-lab-analysis-pipeline`
- `shared-actions-workflows`
- `institutional-automation-scripts`
- `public-research-toolkit`

---

## Quick Recommendations

1. **Start simple.** Use a small number of organizations and expand only when needed.

2. **Use teams everywhere.** Avoid managing access user-by-user.

3. **Prefer organization-owned repositories.** They are easier to govern and preserve when people leave.

4. **Use internal visibility for reusable institutional work.**

5. **Use private visibility for coursework, student work, sensitive research, and institutional systems.**

6. **Choose the identity architecture early.** For the Campus Program, plan around personal GitHub.com accounts with SAML-based authentication.

7. **Do not design around Enterprise Managed Users.** EMU is not available through the Campus Program.

8. **Centralize policy, delegate administration.** Keep enterprise ownership limited and delegate responsibilities through roles and teams.

9. **Design for Campus Program constraints.** Do not assume EMU, data residency, Copilot Business, individual Copilot licenses, expanded Actions usage, Codespaces, paid security products, or Premium Support are included.

10. **Plan add-on product funding separately.** Users can bring their own account entitlements, or the school can fund usage with budgets, payment limits, and an approved payment instrument such as a credit card or eligible Azure Subscription ID.

11. **Use public security documentation for reviews.** Because no NDA is required, plan around publicly available documentation such as SOC 2 and GitHub Trust Center resources.

12. **Document the model.** Create enterprise and organization `README.md` files so users know where work belongs.

---

## Bottom Line

For most schools adopting the Campus Program, the best starting architecture is a **single GitHub Enterprise Cloud enterprise account** with a small set of intentional organizations for teaching, research, institutional IT, open source/community work, and restricted work.

Use **teams** as the primary access model, apply **enterprise-level identity and policy controls** where possible, and grow the structure gradually as governance needs become clearer.

At the same time, schools should design around the specific limitations of the Campus Program:

- Enterprise Managed Users are not available.
- Data residency is not available.
- GitHub Copilot Business is not included.
- Individual Copilot, Actions, and Codespaces entitlements are not automatically granted.
- Paid security products are not included.
- Premium Support is not included.
- NDA-gated security documentation should not be expected.
- Add-on metered products require separate funding, billing setup, or engagement with GitHub Sales.

The most successful Campus Program deployments treat the enterprise account as a governance foundation, while planning separately for paid add-ons, product entitlements, support expectations, security reviews, and institution-specific compliance needs.

---

## Reference Links

- [About GitHub Enterprise Cloud](https://docs.github.com/en/enterprise-cloud@latest/admin/overview/about-github-enterprise-cloud)
- [Best practices for organizing work in your enterprise](https://docs.github.com/en/enterprise-cloud@latest/enterprise-onboarding/setting-up-organizations-and-teams/best-practices)
- [Identity and access management fundamentals](https://docs.github.com/en/enterprise-cloud@latest/admin/concepts/identity-and-access-management/identity-and-access-management-fundamentals)
- [Deciding whether to configure SAML for your enterprise or your organizations](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-iam/using-saml-for-enterprise-iam/deciding-whether-to-configure-saml-for-your-enterprise-or-your-organizations)
- [Using innersource in your enterprise](https://docs.github.com/en/enterprise-cloud@latest/enterprise-onboarding/setting-up-organizations-and-teams/use-innersource)
- [About GitHub Enterprise Cloud with data residency](https://docs.github.com/en/enterprise-cloud@latest/admin/data-residency/about-github-enterprise-cloud-with-data-residency)
- [About GitHub Advanced Security](https://docs.github.com/en/enterprise-cloud@latest/get-started/learning-about-github/about-github-advanced-security)
- [About GitHub Premium Support](https://docs.github.com/en/enterprise-cloud@latest/support/learning-about-github-support/about-github-premium-support)
- [Setting up budgets to control spending on metered products](https://docs.github.com/en/enterprise-cloud@latest/billing/managing-your-billing/using-budgets-control-spending)
- [Supported payment methods for GitHub](https://docs.github.com/en/enterprise-cloud@latest/billing/reference/supported-payment-methods)
- [Azure subscription payments](https://docs.github.com/en/enterprise-cloud@latest/billing/concepts/azure-subscriptions)
- [GitHub Copilot billing through Azure](https://docs.github.com/en/enterprise-cloud@latest/copilot/reference/copilot-billing/azure-billing)
