# Changelog

All notable changes to this project will be documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.1.0] - 2025.10.22

> This release introduces new core upgrade capabilities, AI Personas, security updates and a more refined code base.

### New Features
- **AI Persona**: AI Identity has been replaced with AI Personas. Personas align with typical job titles (eg Accountant) and are lightweight system prompts defining the AI role and behavior, they set expertise, tone, and boundaries. This change was made to better align with industry standards and also integrate with the new AI Tasks. Several hundred new AI Personas have been added spanning many industries and job titles.
- Added Server Version class for single source of truth code and API version.
- Added Database migrations folder to enable upgrading of the database when new versions are deployed.
- Added Client `AllowSignUpsSwitch` component under Settings -> Admin Panel -> Application Settings to allow admins to enable or disable site wide sign ups.

### Changed
- Changed Server entry point script to a modular design for improved maintainability and readability.
- Changed Server entry point script to now support upgrading of the database when new versions are deployed.

### Removed
- Removed AI Prompts, this is now fully manged by the AI and reduces code overhead.
- Removed AI Output Length, this is now fully manged by the AI and reduces code overhead.
- Removed AI Prompt Suggestions, this is now fully manged by the AI and reduces code overhead.

### Security
- Updated i18next to version 25.6.0.
- Updated react-i18next to version 16.1.4.
- Updated node to version 25.0.0.
- Updated @mui/x-data-grid to version 8.14.1.
- Updated @mui/x-date-pickers to version 8.14.1.
- Updated react-router-dom to version 7.9.4
- Updated node to version 24.10.0
- Updated laravel/serializable-closure to version 2.0.6

## [v1.0.0] - 2025.10.04

> The official launch of Athena, your personal life manager, built with privacy, choice and beauty at its heart.

### New Features
- **Platform Foundation**: Launched Client and Server Docker images with a Docker Compose file for easy home deployment.
- **Update Notifications**: Get notified about new Athena releases and see full change logs under Settings -> Admin Panel -> Application software.
- **Application Backup and Restore**: Effortlessly create full or partial backups of your data with a few clicks, stored as transparent, industry-standard SQL dump files. Restore all or specific system components seamlessly, ensuring complete control over your private data.
- **One Click AI Prompts**: Create, edit and use the new AI Prompt Templates for quick and easy AI tasks like summarization, wisdom extraction, writing improvement and more. Located in the Settings -> AI and the AI Chat Input area, simply pick your prompt template and get access to a whole host of refined prompts to get the best answers out of your AI.
- **Athen Mascot!**: With the official launch of Athena comes our new mascot [Thaddeus](docs/mascot.md) the Sheltie!
- Added `createdAt` and `updatedAt` columns to all Database tables, as well as supporting Server and Client code.
- Added Client `Sign Out Everywhere` button to the Settings -> Security -> Active Sessions area.
- Added Client `Prompt Suggestions` switch under Settings -> AI -> AI Assistant area to allow users to enable or disable the feature.
- Added Server `AiProviderType` enum to the `AiProvider` model for better provider classification.
- Added Server background task `sync_ai_providers` to automatically sync all AI Providers for all users nightly so the available models list is up to date.
- Added Server background task `check_updates` to automatically check for new Athena updates and notify the user.

### Changed
- **Greatly Simplified Deployment**: Changed the entire Athena stack to greatly simplify the entire setup process. Users no longer need to manage multiple MariaDB or Nginx config files. The new build process now automatically generates all `.env` and secret variables such as the MariaDB `host/user/passwords`, Server master encryption key and Nginx SSL certificates for each new deployment, greatly enhancing security.
- Changed Client code structure to be in domain-driven folders for improved organization, maintainability and future expansion.
- Changed Client i18n translation keys from `snake_case` to `camelCase` to unify variable naming styles.
- Changed Client `constants` and `types` files to all be `.ts` files for better separation of concerns and code standardization.
- Changed Client AI Chat Settings Popover to display the current AI Model to better align with UX industry standards.
- Changed Client AI Chat Header and Chat History Drawer delete buttons to include a confirmation dialogue when deleting a chat.
- Changed Server code structure to be in domain-driven folders for improved organization, maintainability and future expansion.
- Changed Server `index.php` code by separating the "bootstrapping" of the application container into `bootstrap.php`. This allows cron scripts to access the same services as the main web entry point without code duplication.
- Changed Server code to use named parameters in all constructor and function calls for improved code standardization, readability, maintainability and reduced risk of errors.
- Changed Server Controller code to use a new `HttpStatus` Enum for consistent status handling eg `->withStatus(HttpStatus::OK->value);`.
- Changed Server Repository code to use consistent `save` and `mapToModel` methods.
- Changed Server `AuthController::refresh` function so that the `refreshToken` in the session is rotated instead of revoking the entire session and building a new one.
- Changed Database `ai_message.timestamp` column to use the more standard `created_at` name.
- Changed Database `audit_log.timestamp` column to use the more standard `created_at` name.
- Changed Database `system_log.timestamp` column to use the more standard `created_at` name.
- Changed Database structure so that schema creation and seeding of system data is separated for improved maintainability and clarity.

### Fixed
- Fixed Client `AiSettingsPopover` button that was displayed incorrectly in the AI Chat area when no AI Provider was available.
- Fixed Client issue where after deleting an AI Chat conversation, the `conversationId` wouldn't be cleared properly, causing UI issues.
- Fixed Client issue where tables didn't extend to the edges of a Card when inside a `CardContent` component.
- Fixed Client `AuthProvider` issue where the `refresh` function was causing a cascade of events through  `customFetch`, which then caused a full page refresh of all components every 14 minutes.
- Fixed Server Controller endpoints that were incorrectly returning multiple objects for `create` and `update` actions.
- Fixed Server `delete` and `update` functions to now explicitly verify user ownership of objects before committing changes.
- Fixed Server Controller endpoints response code so a standardized format is used.
- Fixed Server `AuthMiddleware` logging to use the correct `USER_AUTH_FAILURE` LogType.
- Fixed Server `LogController::getAllLogs` issue where warnings were being shown due to improper filter creation.

## [v0.3.4] - 2025.07.28
### New Features
- **Enhanced Profile Options**: Added new fields in **Settings > Profile** for spouse, dependents, occupation, dietary preferences, and detailed address (street, city, state, zip, country). This allows you to enter your personal information in once, and then include it in future AI chats via the new Add Profile Data button.
- **AI Upgrades**:
  - **Prompt Suggestions**: Get ideas to interact with the AI easily.
  - **Add Profile Data**: Choose which personal details to include in AI chats for precise control.
  - **Code & Table Support**: Render code blocks with a "Copy Code" button and display tables clearly in the AI Chat interface.
  - **System Message Handling**: Identity, output length and profile data information is now passed to the AI Assistant by the server for faster and more secure data processing.
  - **Model Type Icons**: See whether your AI model is local or cloud-based with new icons.
  - **Info Tooltips**: Hover over AI Assistant Identity and Output Length settings to view prompt text.
  - **Privacy Alert**: The **Cloud Warning Alert** has been enhanced to highlight risks of cloud-based AI providers, reinforcing local data control.
- **Country Support**: New **country** database table and `/countries` API endpoint for global address formats.
- **Changelog Standardization**: Adopted [Semantic Versioning](https://semver.org/spec/v2.0.0.html) for this changelog to track updates clearly.

### Changed
- **AI Interface**: The AI chat input area is now faster with a beautiful new glow effect when no conversation is selected.
- **Profile Settings Layout**: Reorganized **Settings > Profile** for easier navigation with new components.
- **Currency Support**: Updated the **currency** table to include language translations and defined decimal places for better financial tracking.
- **Account Management**: Moved **Delete Account** to **Settings > Security > Sign In & Account** and renamed **Authentication** to **Sign In & Account** for clarity.
- **Chat Optimization**: Streamlined the **ChatInput** component by moving functionality to **AddProfileDataPopover** and **AiSettingsPopover** for better performance.

### Removed
- **Profile Contact Subcategory**: Removed from the user data structure to simplify the interface.
- **Roadmap File**: Merged `roadmap.md` into this changelog for a single update source.

### Bug Fixes
- Fixed AI Assistant Model Selector issue where identical model names across different AI providers wouldn't work.
- Fixed AI Chat Interface to correctly display AI avatar and model name.
- Fixed AI Chat Interface logo area, which was broken when no AI provider is selected or AI is off.
- Fixed incorrect setting of new user display names.
- Fixed incorrect username validation checking.

## [v0.3.3] - 2025-07-18
### New Features
- **AI Chat Interface**: Introduced a new AI Chat feature in the Client, letting you send and receive messages using your chosen AI model. Customize your experience with AI Assistant options, including different AI Identities and Output Length settings.
- **AI Chat Conversations**: Create, rename, or delete AI chat conversations. You can also let the AI auto-generate conversation titles for convenience.
- **Cloud Privacy Warnings**: Added a **Cloud Warnings** option to alert you when using cloud-based AI providers, helping you stay mindful of your data privacy.
- **AI Toggle**: A new **AI Enabled** setting lets you completely disable all AI features system-wide for maximum control.
- **AI Provider Management**: Manage your AI providers with the new **AI Provider Cards** feature. Add, sync, edit, or remove providers from a supported list, tailored to your preferences.
- **AI Setup Assistant**: A new **AI Setup** component guides you through configuring a new AI provider and selecting your preferred AI model, making setup quick and seamless.

## [v0.2.5] - 2025-05-11
### New Features
- **Secure Authentication**: Added JWT-based authentication with access and refresh tokens for secure Client and Server interactions.
- **HTTPS Support**: Enabled HTTPS with TLS 1.3 and self-signed certificates via Nginx proxy for secure connections.
- **Password Security**: Implemented Argon2id password hashing to safely store user credentials.
