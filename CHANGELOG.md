# Changelog

All notable changes to this project will be documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.2.1] - 2026.06.06

### Fixed
- Docs: Fixed Docker Compose link to be OS agnostic
- Server: Fixed issue in `ImportService` that was causing a database error during any transaction import

## [v1.2.0] - 2026.06.05

> Launch of **Athena Finance**! (Note: Athena Finance **does not** support automated bank syncing)

### Headline New Features
- **Athena Finance:** The official release of Athena Finance, an incredibly private and secure personal finance vault. Track and manage your complete financial life locally with a beautiful, modern and fast user interface. Includes full multi-currency support across accounts, assets, liabilities, portfolios, categories, market data, and powerful insights
- **Dashboard & Accounts:** A comprehensive overview of your entire financial position combined with full lifecycle management of Checking, Savings, Investment, Credit Card, Loan, Retirement and other account types
- **Real-Time Market Data & Exchange Rates:** Integrated real-time and historical data for 50,000+ instruments (stocks, ETFs, mutual funds, cryptocurrencies, bonds, commodities, etc.) across 100+ global exchanges powered by Yahoo Finance
- **Multi-Currency Support:** Automatic conversion at account, asset, and transaction levels, plus 20 new display currencies (AED, CLP, COP, CZK, DKK, EGP, HUF, IDR, ILS, KES, MYR, PHP, PLN, RON, SAR, THB, TWD, UAH, VND, ZAR), taking the total supported to 40 for even broader global coverage.
- **Bulk CSV Transaction Import:** Quickly and easily import tens of thousands of transactions from existing financial institutions using standard banking CSV files. The new import wizard is as powerful as it is beautiful, enabling you to pause and resume imports, precisely configure columns, bulk map categories, easily mark transfer transactions to/from other accounts, auto detect and exclude duplicates and perform a full import roll back in case you make a mistake.
- **AI Chat Streaming:** Enjoy real-time AI chat responses for a smoother, more interactive experience.

### All New Features
- Client: Added new `UserData` component under the new Settings -> Data & Privacy area that enables you to view, download and delete all your data with ease.
- Client: Added `TimePeriodPicker` component to Settings -> Preferences -> Appearance that sets the default time period used for charts (e.g., 1D, 1M, 1Y etc).
- Client: Added `DateFormatPicker` and `TimeFormatPicker` components to Settings -> Preferences -> Localization to allow the user to set their own custom date and time formats application wide.
- Client: Added many new components to support the new Finance -> Accounts, Assets, Categories, Market Data, Symbols and Transaction features.
- Client: Added new components to enable a centralized and standard way of displaying monetary amounts and financial returns application wide. This also includes a conversion tooltip that appears when the currency doesn't match the users display currency, giving exchange rate, date and source information.
- Client: Added `StackedBarChart` component to display 100% stacked bar charts system wide with support for dynamic data, titles, icons, data formating and display options.
- Client: Added `BaseLineChart` component to display beautiful and adaptive line charts system wide with support for dynamic data and Y-axis auto adjusting options.
- Client: Added Tooltips to all `IconButton` components.
- Client: Added localization support to `formatFileSize`.
- Client: Added localization support to MUI Data Grids.
- Client: Added a new `useResponsive` hook to enable centralized mobile and desktop detection.
- Client: Added many new Account, Asset, Asset Lot, Asset Lot Disposition, Asset Value History, Cash Flow, Category, Market Data, Portfolio, Symbol and Transaction hooks to support new Finance features.
- Client: Added many new AI Conversation, Message and Provider hooks and migrated all AI Chat code to use them for improved performance and code maintainability.
- Client: Added `formatCurrency` function that supports displaying Bitcoin, being passed strings for enhanced precision and being given an override currency.
- Client: Added `setPageTitle` utility function and migrated all `<title>` code to ensure centralized and consistent site wide page titles.
- Server: Added many new `/finance/*` API endpoints to support new Finance features.
- Server: Added many new background task scripts to automatically perform nightly tasks such as preloading and updating exchange rates, market data as well as to reconcile user accounts and assets.
- Server: Added country and currency validation logic to `UserSettingService::update`
- Database: Added many new `finance_*` tables to support new Finance features.

### Changed
- Client: A full rework of all translations to align with industry standard [Locize](https://www.locize.com/blog/guide-to-i18n-key-naming) and [ICU Message Format](https://icu.unicode.org/home). This not only enables a better foundation to add more languages in the future, but also ensures a consistent, industry standard for all translations.
- Client: A full rework of all error messages, alerts and snackbars for clearer, standardized user feedback.
- Client: Merged `Application Software`, `Application Backups` and `Application Settings` cards into a single, more refined `Application` card.
- Client: Migrated i18n translation keys for `ai` and `user` to their own domain specific `i18n/*.json` files for improved maintainability.
- Client: Migrated `formatDate` out into separate `formatDate`, `formatTime` and `formatDateTime` functions for clearer code separation, simpler logic and to support the new `DateFormat` and `TimeFormat` user preferences.
- Client: Migrated currency code from `formatNumber` to new `formatCurrency` function for clearer code separation and simpler logic.
- Client: Migrated all `password` TextFields from multiple components to use a new `PasswordInput` primitive.
- Client: Migrated `CurrencyPicker` to a stand alone component that can now be used system wide.
- Client: Migrated all components and typography code in `/styles/theme` out into their own files for better maintainability.
- Client: Migrated all `/domains/common/components` components into a new folder structure to mirror the standard way MUI organizes common components.
- Client: Updated the Admin Panel -> Application Backups stacked bar chart to now render in the users chosen primary color.
- Client: Merged `AddNewAiProviderButton` and `AiProviderEditor` components into a new `AiProviderFormDialog` component.
- Client: Migrated code that deletes AI Providers into a common `DeleteDialog` component.
- Client: Updated the Audit and System log "context" dialogs to match the standard dialog stylings.
- Client: Migrated the Add AI Provider button to the top of the card and updated it to better support mobile screens.
- Client: Updated all Dialogs to better support mobile screens.
- Client: Migrated all Settings -> Profile -> Location components to the `common` domain as new primitives (`StreetAddressInput`, `CityInput`, `CountryPicker` etc) and replaced them with wrapper components (`StreetAddressField`, `CityField`, `LocationCountryPicker` etc). This allows for the primitives to be reused across all domains where ever location inputs are required in the future.
- Client: Migrated all Settings -> AI components to the `ai` domain.
- Client: Renamed multiple Settings components for a more consistent naming standard including: `ActiveSessions` -> `ActiveSessionsList`, `AiSwitch` -> `AiEnabledSwitch`, `UsernameEditor` -> `UsernameField`, `DisplayNameEditor` -> `DisplayNameField`, `DietaryPreferencesEditor` -> `DietaryPreferencesField`, `OccupationEditor` -> `OccupationField`, `DependentsEditor` -> `DependentsField`, `HeightEditor` -> `HeightField`, `ApplicationSettings` -> `ApplicationSettingsDialog`, `AuditLogs` -> `AuditLogsTable`, `SystemLogs` -> `SystemLogsTable`, `UserManagement` -> `UserManagementTable`
- Server: Migrated the `seed` directory to be in the main `/config-defaults/db` folder for a more logical folder structure.
- Server: Separated `/config-defaults/db/init/00-init.sql` into separate files (`00-init.sql`, `01-common.sql`, `02-user.sql` etc) to better align with each domain as well as to improve manageability.
- Server: Updated `/entrypoint/08-setup_mariadb_merged` script to support merging multiple init SQL files.
- Server: Migrated `/Domain/System/Enums/Backup/DataType` enum to `/Domain/System/Enums/AppDomain` and updated it to match all domains including the new AI and Finance. This also ensures all database tables are included in backing up and restoring of data.
- Server: Updated all `MapToModel` and `save` Repository functions to convert all `snake_case` database rows to `camelCase` and vice versa.
- Server: Updated all Finance, Common, AI, System and User domain Server Responses to return Data Transfer Objects (DTOs) for cleaner, more structured and maintainable Controller responses.
- Server: Updated `DatabaseService` and all Service layer code to fully support database transactions where required to ensure atomicity (e.g., using PDO's `beginTransaction()`, `commit()`, and `rollback()`.)
- Server: A full rework of the entire ApiError system and all error messages, giving them more information for better client feedback.
- Database: Migrated all files to `snake_case` naming structure to better align with industry best practices.
- Database: Separated out all migration commands into individual files and updated endpoint script.
- Database: Updated many tables and columns to fix inconsistencies, enable new features and adhere to standards.

### Removed
- None

### Fixed
- Client: Fixed issue where entering values longer than 255 characters in the Settings -> Profile -> Dietary Preferences field would cause a database error.
- Client: Fixed issue where you would see "token expired" error after being on a page for 15+ minutes
- Client: Fixed mobile view issue where the `NavBar` was cutting off content at the bottom of the page.
- Client: Fixed AI Chat not working for the OpenAI provider due to wrong stream handling
- Client: Fixed AI Chat not working for the Anthropic provider due to wrong stream handling
- Client: Fixed the page crashing when opening the options menu in the AI chat history draw
- Client: Fixed the use of `// eslint-disable-next-line...` everywhere due to `customFetch` constantly changing and refreshing everything
- Server: Fixed `UnitSystem` enum, which had incorrect names.
- Database: Fixed `country.region` row enum to be lower case to align with code standards.
- Database: Fixed `ai_persona.category` row enum to be `snake_case` to align with code standards.
- Database: Fixed `created_at` and `updated_at` rows of many tables to ensure consistent `TIMESTAMP` usage and `NOT NULL` constraints.

### Security
- Client: Updated Node.js docker image from `24` to `26`
- Client: Added `@mui/x-charts` at version `9.3.0`
- Client: Added `i18next-icu` at version `2.4.3`
- Client: Updated `@mui/material` from `7.3.4` to `9.0.1`
- Client: Updated `@mui/x-data-grid` from `8.14.1` to `9.3.0`
- Client: Updated `@mui/x-date-pickers` from `8.14.1` to `9.3.0`
- Client: Updated `date-fns` from `4.1.0` to `4.4.0`
- Client: Updated `i18next` from `25.6.0` to `26.3.0`
- Client: Updated `i18next-browser-languagedetector` from `8.2.0` to `8.2.1`
- Client: Updated `react` from `19.2.0` to `19.2.7`
- Client: Updated `react-dom` from `19.2.0` to `19.2.7`
- Client: Updated `react-i18next` from `16.1.4` to `17.0.8`
- Client: Updated `react-router-dom` from `7.9.4` to `7.16.0`
- Server: Updated PHP docker image from `8.4.12` to `8.5.6`  
- Server: Updated `symfony/translation` from `7.3.4` to `8.1.0`  
- Server: Updated `symfony/translation-contracts` from `3.6.0` to `3.7.0`  
- Server: Updated `symfony/polyfill-mbstring` from `1.33.0` to `1.38.1`
- Server: Updated `slim/psr7` from `1.7.1` to `1.8.0`
- Server: Updated `slim/slim` from `4.15.0` to `4.15.2`

## [v1.1.0] - 2025.10.22

> This release introduces new core upgrade capabilities, AI Personas and security updates.

### New Features
- **AI Persona**: AI Identity has been replaced with AI Personas. Personas align with typical job titles (eg Accountant) and are lightweight system prompts defining the AI role and behavior, they set expertise, tone, and boundaries. This change was made to better align with industry standards and several hundred new AI Personas have been added spanning many industries and job titles.
- Added Server Version class for single source of truth code and API version.
- Added Database migrations folder to enable upgrading of the database when new versions are deployed.
- Added Client `AllowSignUpsSwitch` component under Settings -> Admin Panel -> Application Settings -> General to allow admins to enable or disable site wide sign ups.

### Changed
- Changed Server entry point script to a modular design for improved maintainability and readability.
- Changed Server entry point script to now support upgrading of the database when new versions are deployed.

### Removed
- Removed AI Prompts, this is now fully manged by the AI and reduces code overhead.
- Removed AI Output Length, this is now fully manged by the AI and reduces code overhead.
- Removed AI Prompt Suggestions, this is now fully manged by the AI and reduces code overhead.

### Security
- Updated `i18next` to version 25.6.0
- Updated `react-i18next` to version 16.1.4
- Updated `node` to version 25.0.0
- Updated `@mui/x-data-grid` to version 8.14.1
- Updated `@mui/x-date-pickers` to version 8.14.1
- Updated `react-router-dom` to version 7.9.4
- Updated `node` to version 24.10.0
- Updated `laravel/serializable-closure` to version 2.0.6

## [v1.0.0] - 2025.10.04

> The official launch of Athena, your personal life manager, built with privacy, choice and beauty at its heart.

### New Features
- **Platform Foundation**: Launched Client and Server Docker images with a Docker Compose file for easy home deployment.
- **Update Notifications**: Get notified about new Athena releases and see full change logs under Settings -> Admin Panel -> Application software.
- **Application Backup and Restore**: Effortlessly create full or partial backups of your data with a few clicks, stored as transparent, industry-standard SQL dump files. Restore all or specific system components seamlessly, ensuring complete control over your private data.
- **One Click AI Prompts**: Create, edit and use the new AI Prompt Templates for quick and easy AI tasks like summarization, wisdom extraction, writing improvement and more. Located in the Settings -> AI and the AI Chat Input area, simply pick your prompt template and get access to a whole host of refined prompts to get the best answers out of your AI.
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
- Fixed Client `ChatSettingsPopover` button that was displayed incorrectly in the AI Chat area when no AI Provider was available.
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
- **Chat Optimization**: Streamlined the **ChatInput** component by moving functionality to **ChatAddProfileDataPopover** and **ChatSettingsPopover** for better performance.

### Removed
- **Profile Contact Subcategory**: Removed from the user data structure to simplify the interface.
- **Roadmap File**: Merged `roadmap.md` into this changelog for a single update source.

### Bug Fixes
- Fixed AI Assistant Model Selector issue where identical model names across different AI providers wouldn't work.
- Fixed AI Chat Interface to correctly display AI avatar and model name.
- Fixed AI Chat Interface logo area, which was broken when no AI provider is selected or AI is off.
- Fixed incorrect setting of new user display names.
- Fixed incorrect username validation checking.

## [v0.3.3] - 2025.07.18
### New Features
- **AI Chat Interface**: Introduced a new AI Chat feature in the Client, letting you send and receive messages using your chosen AI model. Customize your experience with AI Assistant options, including different AI Identities and Output Length settings.
- **AI Chat Conversations**: Create, rename, or delete AI chat conversations. You can also let the AI auto-generate conversation titles for convenience.
- **Cloud Privacy Warnings**: Added a **Cloud Warnings** option to alert you when using cloud-based AI providers, helping you stay mindful of your data privacy.
- **AI Toggle**: A new **AI Enabled** setting lets you completely disable all AI features system-wide for maximum control.
- **AI Provider Management**: Manage your AI providers with the new **AI Provider Cards** feature. Add, sync, edit, or remove providers from a supported list, tailored to your preferences.
- **AI Setup Assistant**: A new **AI Setup** component guides you through configuring a new AI provider and selecting your preferred AI model, making setup quick and seamless.

## [v0.2.5] - 2025.05.11
### New Features
- **Secure Authentication**: Added JWT-based authentication with access and refresh tokens for secure Client and Server interactions.
- **HTTPS Support**: Enabled HTTPS with TLS 1.3 and self-signed certificates via Nginx proxy for secure connections.
- **Password Security**: Implemented Argon2id password hashing to safely store user credentials.
