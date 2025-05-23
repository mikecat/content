---
title: "TrustedScriptURL: toString() method"
short-title: toString()
slug: Web/API/TrustedScriptURL/toString
page-type: web-api-instance-method
browser-compat: api.TrustedScriptURL.toString
---

{{APIRef("Trusted Types API")}}{{AvailableInWorkers}}

The **`toString()`** method of the {{domxref("TrustedScriptURL")}} interface returns a string which may safely inserted into an [injection sink](/en-US/docs/Web/API/Trusted_Types_API#concepts_and_usage).

## Syntax

```js-nolint
toString()
```

### Parameters

None.

### Return value

A string containing the sanitized URL

## Examples

The constant `sanitized` is an object created via a Trusted Types policy. The `toString()` method returns a string to safely use to load a third party script.

```js
const sanitized = scriptPolicy.createScriptURL(
  "https://example.com/my-script.js",
);
console.log(sanitized.toString());
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}
