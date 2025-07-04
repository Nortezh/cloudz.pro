# Commit Guide: Conventional Commits + Gitmoji

A comprehensive guide for creating meaningful commit messages using Conventional Commits specification with Gitmoji emojis.

## Commit Pattern Structure

```
<type>: <gitmoji> <description>

<body with bullet points>
```

**Example:**
```
feat: ✨ add user authentication

- Implement JWT token validation
- Add login/logout endpoints
- Create user session management
```

---

## Conventional Commit Types

### Core Types (Most Common)

| Type | Description | Usage |
|------|-------------|-------|
| `feat` | A new feature for the user | New functionality |
| `fix` | A bug fix for the user | Bug fixes |
| `docs` | Documentation only changes | README, comments, etc. |
| `style` | Code formatting changes | Whitespace, semicolons, etc. |
| `refactor` | Code restructuring | No new features or bug fixes |
| `test` | Adding or updating tests | Unit tests, integration tests |
| `chore` | Build process or tool changes | Dependencies, configs |

### Extended Types

| Type | Description | Usage |
|------|-------------|-------|
| `perf` | Performance improvements | Optimization |
| `ci` | CI configuration changes | GitHub Actions, etc. |
| `build` | Build system changes | Webpack, npm scripts |
| `revert` | Revert previous commit | Undo changes |

### Optional Types

| Type | Description | Usage |
|------|-------------|-------|
| `security` | Security improvements | Vulnerability fixes |
| `deps` | Dependency updates | Package updates |
| `config` | Configuration changes | Settings, env files |
| `release` | Release commits | Version bumps |
| `hotfix` | Critical urgent fixes | Production issues |

**Total: 16 commit types**

---

## Complete Gitmoji Reference

### Code Structure & Quality
| Emoji | Code | Description |
|-------|------|-------------|
| 🎨 | `:art:` | Improve structure / format of the code |
| ♻️ | `:recycle:` | Refactor code |
| 💩 | `:poop:` | Write bad code that needs to be improved |
| ⚰️ | `:coffin:` | Remove dead code |
| 🧑‍💻 | `:technologist:` | Improve developer experience |

### Performance & Optimization
| Emoji | Code | Description |
|-------|------|-------------|
| ⚡️ | `:zap:` | Improve performance |
| 📈 | `:chart_with_upwards_trend:` | Add or update analytics or track code |
| 🔍️ | `:mag:` | Improve SEO |

### Features & Functionality
| Emoji | Code | Description |
|-------|------|-------------|
| ✨ | `:sparkles:` | Introduce new features |
| 🎉 | `:tada:` | Begin a project |
| 🚀 | `:rocket:` | Deploy stuff |
| 💫 | `:dizzy:` | Add or update animations and transitions |
| 🏗️ | `:building_construction:` | Make architectural changes |

### Bug Fixes & Issues
| Emoji | Code | Description |
|-------|------|-------------|
| 🐛 | `:bug:` | Fix a bug |
| 🚑️ | `:ambulance:` | Critical hotfix |
| 🩹 | `:adhesive_bandage:` | Simple fix for a non-critical issue |
| 🥅 | `:goal_net:` | Catch errors |

### UI/UX & Design
| Emoji | Code | Description |
|-------|------|-------------|
| 💄 | `:lipstick:` | Add or update the UI and style files |
| 📱 | `:iphone:` | Work on responsive design |
| 🚸 | `:children_crossing:` | Improve user experience / usability |
| ♿️ | `:wheelchair:` | Improve accessibility |

### Documentation & Comments
| Emoji | Code | Description |
|-------|------|-------------|
| 📝 | `:memo:` | Add or update documentation |
| 💡 | `:bulb:` | Add or update comments in source code |
| 💬 | `:speech_balloon:` | Add or update text and literals |

### Testing
| Emoji | Code | Description |
|-------|------|-------------|
| ✅ | `:white_check_mark:` | Add, update, or pass tests |
| 🧪 | `:test_tube:` | Add a failing test |
| 🤡 | `:clown_face:` | Mock things |
| 📸 | `:camera_flash:` | Add or update snapshots |

### Security & Privacy
| Emoji | Code | Description |
|-------|------|-------------|
| 🔒️ | `:lock:` | Fix security or privacy issues |
| 🔐 | `:closed_lock_with_key:` | Add or update secrets |
| 🛂 | `:passport_control:` | Work on code related to authorization, roles and permissions |

### Dependencies & Packages
| Emoji | Code | Description |
|-------|------|-------------|
| ➕ | `:heavy_plus_sign:` | Add a dependency |
| ➖ | `:heavy_minus_sign:` | Remove a dependency |
| ⬆️ | `:arrow_up:` | Upgrade dependencies |
| ⬇️ | `:arrow_down:` | Downgrade dependencies |
| 📌 | `:pushpin:` | Pin dependencies to specific versions |
| 📦️ | `:package:` | Add or update compiled files or packages |

### Configuration & Setup
| Emoji | Code | Description |
|-------|------|-------------|
| 🔧 | `:wrench:` | Add or update configuration files |
| 🔨 | `:hammer:` | Add or update development scripts |
| 🙈 | `:see_no_evil:` | Add or update a .gitignore file |
| 🧱 | `:bricks:` | Infrastructure related changes |

### CI/CD & Build
| Emoji | Code | Description |
|-------|------|-------------|
| 👷 | `:construction_worker:` | Add or update CI build system |
| 💚 | `:green_heart:` | Fix CI Build |
| 🚧 | `:construction:` | Work in progress |

### Version Control & Releases
| Emoji | Code | Description |
|-------|------|-------------|
| 🔖 | `:bookmark:` | Release / Version tags |
| ⏪️ | `:rewind:` | Revert changes |
| 🔀 | `:twisted_rightwards_arrows:` | Merge branches |
| 🚚 | `:truck:` | Move or rename resources (e.g.: files, paths, routes) |

### Data & Database
| Emoji | Code | Description |
|-------|------|-------------|
| 🗃️ | `:card_file_box:` | Perform database related changes |
| 🌱 | `:seedling:` | Add or update seed files |
| 🧐 | `:monocle_face:` | Data exploration/inspection |

### Logging & Monitoring
| Emoji | Code | Description |
|-------|------|-------------|
| 🔊 | `:loud_sound:` | Add or update logs |
| 🔇 | `:mute:` | Remove logs |
| 🩺 | `:stethoscope:` | Add or update healthcheck |

### File Operations
| Emoji | Code | Description |
|-------|------|-------------|
| 🔥 | `:fire:` | Remove code or files |
| 📄 | `:page_facing_up:` | Add or update license |
| 🍱 | `:bento:` | Add or update assets |
| 🗑️ | `:wastebasket:` | Deprecate code that needs to be cleaned up |

### Warnings & Alerts
| Emoji | Code | Description |
|-------|------|-------------|
| 🚨 | `:rotating_light:` | Fix compiler / linter warnings |
| 🚩 | `:triangular_flag_on_post:` | Add, update, or remove feature flags |

### Breaking Changes
| Emoji | Code | Description |
|-------|------|-------------|
| 💥 | `:boom:` | Introduce breaking changes |

### Contributors & Team
| Emoji | Code | Description |
|-------|------|-------------|
| 👥 | `:busts_in_silhouette:` | Add or update contributor(s) |

### Business & Finance
| Emoji | Code | Description |
|-------|------|-------------|
| 👔 | `:necktie:` | Add or update business logic |
| 💸 | `:money_with_wings:` | Add sponsorships or money related infrastructure |

### Text & Content
| Emoji | Code | Description |
|-------|------|-------------|
| ✏️ | `:pencil2:` | Fix typos |
| 🏷️ | `:label:` | Add or update types |

### Special & Experimental
| Emoji | Code | Description |
|-------|------|-------------|
| 🥚 | `:egg:` | Add or update an easter egg |
| ⚗️ | `:alembic:` | Perform experiments |
| 🍻 | `:beers:` | Write code drunkenly |
| 🧵 | `:thread:` | Add or update code related to multithreading or concurrency |
| 🦺 | `:safety_vest:` | Add or update code related to validation |
| ✈️ | `:airplane:` | Improve offline support |

### Internationalization & External APIs
| Emoji | Code | Description |
|-------|------|-------------|
| 🌐 | `:globe_with_meridians:` | Internationalization and localization |
| 👽️ | `:alien:` | Update code due to external API changes |

---

## Common Commit Examples

### Feature Development
```bash
feat: ✨ add user registration system
feat: 🎉 initialize new project
feat: 🚀 deploy to production
```

### Bug Fixes
```bash
fix: 🐛 resolve login validation error
fix: 🚑️ critical security vulnerability patch
fix: 🩹 minor styling issue on mobile
```

### Code Quality
```bash
refactor: ♻️ restructure authentication service
style: 🎨 improve code formatting
perf: ⚡️ optimize database queries
```

### Documentation & Testing
```bash
docs: 📝 update API documentation
test: ✅ add unit tests for user service
test: 🧪 add failing test for edge case
```

### Dependencies & Configuration
```bash
chore: ➕ add lodash dependency
chore: ⬆️ upgrade React to v18
chore: 🔧 update webpack configuration
```

### CI/CD & Build
```bash
ci: 👷 add GitHub Actions workflow
build: 📦️ update build process
ci: 💚 fix CI build issues
```

---

## Quick Reference

**Total Gitmoji Emojis:** 64  
**Total Conventional Commit Types:** 16  
**Most Common Types:** `feat`, `fix`, `docs`, `chore`, `refactor`  
**Most Common Emojis:** ✨, 🐛, 📝, 🔧, ♻️

---

## Resources

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Gitmoji](https://gitmoji.dev/)
- [Semantic Versioning](https://semver.org/)

---

*This guide helps maintain consistent, meaningful commit messages that improve project history readability and enable automated tooling.* 