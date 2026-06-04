# Localization Guide

Athena supports multiple languages to make the platform accessible globally. This guide explains our standards as well as how to add or update translations.

## Table of Contents
- [Overview](#overview)
- [File Structure](#file-structure)
- [Internationalization Usage](#internationalization-usage)
- [Client Translation Process](#client-translation-process)
- [Server Translation Process](#server-translation-process)

## Overview

### Client
- Uses `i18next`, `react-i18next` and `i18next-icu` libraries and follows strict guidelines as recommended by [Locize](https://www.locize.com/blog/guide-to-i18n-key-naming)
- Translation keys are stored in their respective domain translation file (e.g., [athena-client/src/domains/common/i18n/](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/i18n) for the `common` domain)
- Translation keys never have raw HTML embedded in them
- Translation keys use `camelCase` and the singular form (e.g., `account.label.balance`)
- Translation keys use structured (semantic) naming and align to `<feature>.<bucket>.<key>` (e.g., `account.action.add`)
  - **Feature:** Describes WHERE the string is used, aligns with a domain's sub-area or business logic (e.g., `setting`, `auth`, `profile` etc)
  - **Bucket:** Describes HOW the string is used, aligns with a general category (e.g., `action`, `message`, `error`, `label` etc)
  - **Key:** Describes WHAT the string is, should be descriptive enough to give context to a translator, but without verbosity (e.g., `balance`, `exchangeRate`, `transactionDeleteSuccess` etc)
- Translation keys should nest a maximum of 2-3 levels deep
- Translation keys should only use interpolation when the inserted data is truly dynamic (e.g., user input or computed data)
- Translation keys should never be reused. Create a unique key for every unique instance of a string, even for things like `Add <object>`
- Only strictly governed strings (e.g., country names, basic actions, months etc) are allowed in the `common` domain translation file and can be reused
- Pluralization uses the industry standard ICU Message Format, which embeds logic directly into the translation string
```json
{
  "ai": {
    "label": {
      "modelsAvailable": "{count, plural, one {# model available} other {# models available}}",
    }
  }
}
```
```typescript
const { t: tAi } = useTranslation('ai');
tAi('ai.label.modelsAvailable', { count: provider.models.length })
```
- The `t` object should always be renamed to the domain to ensure full type safety
- When accessing multiple namespaces, use multiple `t` objects, this maintains type safety and increases clarity
```typescript
const { t: tCommon } = useTranslation('common');
const { t: tFinance } = useTranslation('finance');
tFinance('account.field.accountType')
tCommon('action.save')
```

### Server
- Uses `symfony/translation` with the translations stored in PHP files located under [athena-server/src/Translations](https://github.com/athena-alpha/athena-server/blob/main/src/Translations)


## File Structure
Translation files are located in the below directories:
- **Client:**
  - [athena-client/src/domains/ai/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/ai/i18n)
  - [athena-client/src/domains/common/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/i18n)
  - [athena-client/src/domains/finance/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/finance/i18n)
  - [athena-client/src/domains/health/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/health/i18n)
  - [athena-client/src/domains/strategy/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/strategy/i18n)
  - [athena-client/src/domains/user/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/user/i18n)
- **Server:** [athena-server/src/Translations](https://github.com/athena-alpha/athena-server/blob/main/src/Translations)

On the Client side there are different JSON files in each domains i18n folder. One is the `domains/common/i18n/en-US.json` translation file for all `common` functions (e.g., buttons, date, time, units etc), the others are for each domain such as `domains/finance/i18n/en-US.json` or `domains/user/i18n/en-US.json`. These only contain translations specific to that domain

The server only has a single `core.en.php` translation file for each languge

```
.
├── athena-client/
│   └── src/
│       └── domains/
│           |── common/
│           |   └── i18n/
│           |       ├── en-US.json
│           |       ├── fr-FR.json
│           |       └── ...
│           └── finance/
│               └── i18n/
│                   ├── en-US.json
│                   ├── fr-FR.json
│                   └── ...
└── athena-server/
    └── src/
        └── Translations/
            ├── core.en.php
            ├── core.fr.php
            └── ...
```

## Internationalization Usage
There are a number of built in utility functions to support easily displaying common things that fully support internationalization.

### Formating Date & Time
When using [formatDate](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/dateTime.utils.ts), [formatTime](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/dateTime.utils.ts) and [formatDateTime](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/dateTime.utils.ts), make sure you include the users personal preferences via `user`.

```typescript
import { useAuth } from '@/domains/user/hooks';
import { formatDate, formatTime, formatDateTime } from '@/domains/common/utils';

const { user } = useAuth(); // (en-US, America/New York)

// Pass a ISO 8601 format (YYYY-MM-DDTHH:MM:SS+00:00)

formatDate(
  '2026-12-31T09:15:00+00:00',
  user?.preferences?.localization?.dateFormat,
  user?.preferences?.localization?.timeZone,
);
// '12/31/2026'

formatTime(
  '2026-12-31T09:15:00+00:00',
  user?.preferences?.localization?.timeFormat,
  user?.preferences?.localization?.timeZone,
);
// '9:15 AM' 

formatDateTime(
  '2026-12-31T09:15:00+00:00',
  user?.preferences?.localization?.dateFormat,
  user?.preferences?.localization?.timeFormat,
  user?.preferences?.localization?.timeZone,
);
// '12/31/2026, 9:15 AM'
```

### Formatting Numbers
[formatNumber](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/number.utils.ts) accepts pre-built [numberFormats](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/intl.constants.ts) options depending on the type of data you're displaying.

```typescript
import { formatNumber } from '@/domains/common/utils';
import { numberFormats } from '@/domains/common/constants';

formatNumber(1234.567, numberFormats.decimal);
// '1,234.57'

formatNumber(0.456, numberFormats.percent);
// '45.60%'

formatNumber(1234.567, numberFormats.integer);
// '1,235'

formatNumber(1234.567, numberFormats.significant12);
// '1,235.567000000000'
```

### Formatting Currency
[formatCurrency](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/number.utils.ts) supports numerous currencies including Bitcoin, which requires extra support for its 8 decimal places of required accuracy.

```typescript
import { formatCurrency } from '@/domains/common/utils';

formatCurrency(1234.567, 'NZD');
// 'NZ$1,234.57'

formatCurrency(0.12345678, 'BTC');
// '₿0.1234578'

formatCurrency(0.12345678, 'BTC', { maximumFractionDigits: 2 });
// '₿0.12'
```

### Formatting File Size
[formatFileSize](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/number.utils.ts) supports up to exabyte units as well as an optional fixed decimal places number.

```typescript
import { formatFileSize } from '@/domains/common/utils';

formatFileSize(1024);
// '1 KB'

formatFileSize(1048576, 2);
// '1.00 MB'
```

## Client Translation Process
1. In each of the domain `i18n` folders, copy and paste one of the other language files such as `en-US.json` as a starting point, then rename it to your language code (e.g., `en-GB.json`)
2. Enter in the new language translations for each line. Make sure to keep the key words (e.g., `nav` or `home`) as well as any [interpolation value](https://www.i18next.com/translation-function/interpolation) (the values inside { } brackets) untouched
```json
{
  "branding": {
    "slogan": "Your personal life manager, built with privacy, choice and beauty at its heart."
  },
  "nav": {
    "home": "Home"
  },
  "welcome": {
    "hi": "Hi {name}!",
  },
  ...
  ..
  .
}
```
3. Add your new language to the `supportedLanguages` global constant list in the [i18n.constants.ts](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/i18n.constants.ts) file to make it available in the Localization language selector. The object is made up of:
- code: [IETF BCP 47 Language Tag](https://en.wikipedia.org/wiki/IETF_language_tag)
- label: Language name presented in its own language
- flagCode: [ISO 3166-1 Alpha-2 Codes](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)
```typescript
export const supportedLanguages = {
  enUS: { code: 'en-US', label: 'English (United States)', flagCode: 'us' },
  frFR: { code: 'fr-FR', label: 'Français (France)', flagCode: 'fr' },
} as const;
```
4. Add your new language to the `dateFnsLocales` global constant list in the [i18n.constants.ts](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/i18n.constants.ts) file to make it available in all `DatePicker` and `TimePicker` components. The object is made up of:
- [IETF BCP 47 Language Tag](https://en.wikipedia.org/wiki/IETF_language_tag)
- [date-fns locale](https://github.com/date-fns/date-fns/tree/main/src/locale)
```typescript
import { enUS } from 'date-fns/locale';

export const dateFnsLocales = {
  'en-US': enUS,
} as const;
```
5. Add your new language to the `gridLocales` global constant list in the [i18n.constants.ts](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/i18n.constants.ts) file to make it available in all `DataGrid` components. The object is made up of:
- [IETF BCP 47 Language Tag](https://en.wikipedia.org/wiki/IETF_language_tag)
- [x-data-grid locale](https://github.com/mui/mui-x/tree/HEAD/packages/x-data-grid/src/locales)
```typescript
import { enUS as gridEnUS } from '@mui/x-data-grid/locales';

export const gridLocales = {
  'en-US': gridEnUS,
} as const;
```
6. Add your new language to the `translationResources` global constant in the [i18n.constants.ts](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/i18n.constants.ts) file to make the translation files available to i18n. Import each of the domains specific .json files, giving them an appropriate name, then add on a new `en-GB` object copying the existing format
```typescript
import enUsAi from '@/domains/ai/i18n/en-US.json';
import enUsCommon from '@/domains/common/i18n/en-US.json';
import enUsFinance from '@/domains/finance/i18n/en-US.json';
import enUsHealth from '@/domains/health/i18n/en-US.json';
import enUsStrategy from '@/domains/strategy/i18n/en-US.json';
import enUsUser from '@/domains/user/i18n/en-US.json';

export const translationResources = {
  'en-US': {
    ai: enUsAi,
    common: enUsCommon,
    finance: enUsFinance,
    health: enUsHealth,
    strategy: enUsStrategy,
    user: enUsUser,
  },
} as const;
```

## Server Translation Process
1. In [athena-server/src/Translations](https://github.com/athena-alpha/athena-server/blob/main/src/Translations), copy and paste one of the other language file groups as a starting point, then rename them to your language code (e.g., `core.es.php`)
2. Enter in the new language translations for each line. Make sure to keep the key words (e.g., `dbQueryFailed`) as well as any [interpolation value](https://www.i18next.com/translation-function/interpolation) (the values inside {{ }} brackets) untouched
```php
return [
  // General Errors
  'dbQueryFailed' => 'Database query failed to execute, please see Server log details',
  'forbidden' => 'Access to this resource is forbidden',
  'unauthorized' => 'Authentication required',
  ...
  ..
  .
]
```
