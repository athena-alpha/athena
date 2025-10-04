# Localization Guide

Athena supports multiple languages to make the platform accessible globally. This guide explains how to add or update translations.

## Table of Contents
- [Overview](#overview)
- [File Structure](#file-structure)
- [Internationalization Usage](#internationalization-usage)
- [Client Translation Process](#client-translation-process)
- [Server Translation Process](#server-translation-process)

## Overview
**Domains:** We follow Domain-Driven Design (DDD) to help ensure modular and easily maintainable code. This means all code is separated into distinct domains such as `common`, `users`, `finance` etc. Language translation files also follow this pattern with translations for a specific domain located in that domains `i18n` folder such as `domains/common/i18n/en-US.json` and `domains/users/i18n/en-US.json`.

### Client
- Uses `i18next` and `react-i18next` with translations stored in JSON files located under the appropriate domain, eg. [athena-client/src/domains/common/i18n/](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/i18n) for the `common` domain

### Server
- Uses `symfony/translation` with the translations stored in PHP files located under [athena-server/src/Translations](https://github.com/athena-alpha/athena-server/blob/main/src/Translations)


## File Structure
Translation files are located in the below directories:
- **Client:**
  - [athena-client/src/domains/common/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/i18n)
  - [athena-client/src/domains/strategy/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/strategy/i18n)
  - [athena-client/src/domains/health/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/health/i18n)
  - [athena-client/src/domains/finance/i18n](https://github.com/athena-alpha/athena-client/blob/main/src/domains/finance/i18n)
- **Server:** [athena-server/src/Translations](https://github.com/athena-alpha/athena-server/blob/main/src/Translations)

On the Client side there are different JSON files in each domains i18n folder. One is the `domains/common/i18n/en-US.json` translation file for all common functions (eg. buttons, menus etc), the others are for each domain such as `domains/finance/i18n/en-US.json` or `domains/health/i18n/en-US.json`. These only contain translations specific to that domain

The server only has a single `core.en.php` translation file for each languge.

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
│           └── strategy/
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
When displaying time, date, numeric or currency data, make sure you display it using the users personal preferences. Use the in built [formatDate](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/dateUtils.ts) and [formateNumber](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/utils/numberUtils.ts) utility functions, pick your format (via the [numberFormats](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/intl.constants.ts) and [dateTimeFormats](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/intl.constants.ts) options) and pass it the `user` object.

```typescript
import { useAuth } from '@/domains/users/hooks';
import { formatNumber, formatDate } from '@/domains/common/utils';
import { numberFormats, dateTimeFormats } from '@/domains/common/constants';

const { user } = useAuth(); // (en-AU, Australia/Sydney, AUD)

// Pass a ISO 8601 format (YYYY-MM-DDTHH:MM:SS+00:00)

formatDate('2023-10-01T12:00:00+00:00', dateTimeFormats.dateTime.short, user);
// '1 Oct 2023, 12:00 pm'

formatDate('2023-10-01T12:00:00+00:00', dateTimeFormats.date.short, user);
// '1 Oct 2023' 

formatDate('2023-10-01T12:00:00+00:00', dateTimeFormats.time.short, user);
// '12:00 PM'

formatNumber(1234.567, numberFormats.decimal, user);
// '1,234.57'

formatNumber(1234.567, numberFormats.currency, user);
// '$1,234.57'

formatNumber(0.456, numberFormats.percent, user);
// '45.6%'

formatNumber(1234.567, numberFormats.integer, user);
// '1,235'
```

## Client Translation Process
1. In each of the domain `i18n` folders, copy and paste one of the other language files such as `en-US.json` as a starting point, then rename it to your language code (eg. `en-GB.json`)
2. Enter in the new language translations for each line. Make sure to keep the key words (eg. `nav` or `home`) as well as any [interpolation value](https://www.i18next.com/translation-function/interpolation) (the values inside {{ }} brackets) untouched
```json
{
  "branding": {
    "slogan": "Your personal life manager, built with privacy, choice and beauty at its heart."
  },
  "nav": {
    "home": "Home"
  },
  "welcome": {
    "hi": "Hi {{name}}!",
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
5. Add your new language to the `translationResources` global constant in the [i18n.constants.ts](https://github.com/athena-alpha/athena-client/blob/main/src/domains/common/constants/i18n.constants.ts) file to make the translation files available to i18n. Import each of the domains specific .json files, giving them an appropriate name, then add on a new `en-GB` object copying the existing format
```typescript
import enUsCommon from '@/domains/common/i18n/en-US.json';
import enUsStrategy from '@/domains/strategy/i18n/en-US.json';
import enUsHealth from '@/domains/health/i18n/en-US.json';
import enUsFinance from '@/domains/finance/i18n/en-US.json';

export const translationResources = {
  'en-US': {
    common: enUsCommon,
    strategy: enUsStrategy,
    health: enUsHealth,
    finance: enUsFinance,
  },
} as const;
```

## Server Translation Process
1. In [athena-server/src/Translations](https://github.com/athena-alpha/athena-server/blob/main/src/Translations), copy and paste one of the other language file groups as a starting point, then rename them to your language code (eg. `core.es.php`)
2. Enter in the new language translations for each line. Make sure to keep the key words (eg. `db_query_failed`) as well as any [interpolation value](https://www.i18next.com/translation-function/interpolation) (the values inside {{ }} brackets) untouched
```php
return [
  // General Errors
  'db_query_failed' => 'Database query failed to execute, please see Server log details',
  'forbidden' => 'Access to this resource is forbidden',
  'unauthorized' => 'Authentication required',
  ...
  ..
  .
]
```
