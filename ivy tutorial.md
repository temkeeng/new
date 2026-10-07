# Ivy Tutors Network --- GTM + Google Ads Purchase Tracking

## Goal

Track successful purchases that arrive at:

`https://ivytutorsnetwork.com/enrolled`

in Google Ads through Google Tag Manager.

### IDs

-   **GTM container:** `GTM-MCKF7VXW`
-   **Google Ads Conversion ID:** `1060973099`
-   **Google Ads Conversion Label:** `RaPBCOvCg4QdEKvU9PkD`
-   **Currency:** `USD`

Plans:

  plan                value
  ----------------- -------
  `group`               699
  `online-tutor`       1299
  `at-home-tutor`      1499

------------------------------------------------------------------------

## 1. Install GTM on `ivytutorsnetwork.com`

Add this as high as possible inside `<head>` on every page:

``` html
<!-- Google Tag Manager -->
<script>
(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-MCKF7VXW');
</script>
<!-- End Google Tag Manager -->
```

Add this immediately after the opening `<body>` tag:

``` html
<!-- Google Tag Manager (noscript) -->
<noscript><iframe
src="https://www.googletagmanager.com/ns.html?id=GTM-MCKF7VXW"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->
```

------------------------------------------------------------------------

## 2. Fire the purchase event on `/enrolled`

**Important:** fire this only after the backend has confirmed that the
purchase is actually successful.

Push the verified purchase into `dataLayer`:

``` html
<script>
window.dataLayer = window.dataLayer || [];

window.dataLayer.push({
  event: 'purchase',
  plan: VERIFIED_PURCHASE.plan,
  value: Number(VERIFIED_PURCHASE.value),
  currency: 'USD',
  transaction_id: VERIFIED_PURCHASE.transactionId
});
</script>
```

`VERIFIED_PURCHASE` above is a placeholder for the site's actual
verified order/payment object. Replace those three expressions with the
real values available in the application.

Expected values:

``` text
plan: group | online-tutor | at-home-tutor
value: 699 | 1299 | 1499
currency: USD
transaction_id: unique ID for the completed order/payment
```

### Do not trust URL parameters as proof of payment

The redirect may contain `plan`, `value`, or a session/order identifier.
Do not fire the purchase event merely because someone visits
`/enrolled?plan=group&value=699`.

Verify the completed purchase server-side first, then populate the event
with the verified values.

The `transaction_id` must be stable and unique for the purchase so
Google Ads can deduplicate repeat page loads where applicable.

------------------------------------------------------------------------

## 3. GTM event contract

Once the implementation is live, GTM should receive exactly this shape:

``` javascript
{
  event: 'purchase',
  plan: 'group',
  value: 699,
  currency: 'USD',
  transaction_id: 'UNIQUE_ORDER_OR_PAYMENT_ID'
}
```

The same structure applies to the other two plans with their
corresponding plan/value.

------------------------------------------------------------------------

## 4. Google Ads conversion

The GTM web container should fire the Google Ads Conversion Tracking tag
when the custom event `purchase` occurs.

Use:

``` text
Conversion ID: 1060973099
Conversion Label: RaPBCOvCg4QdEKvU9PkD
Value: dataLayer `value`
Currency: dataLayer `currency`
Transaction ID: dataLayer `transaction_id`
Trigger: Custom Event = purchase
```

If this GTM configuration is being handled separately, Andrew only needs
to complete **Steps 1--3**.

------------------------------------------------------------------------

## 5. Test before production sign-off

Using GTM Preview / Tag Assistant:

1.  Complete a test purchase.
2.  Confirm the customer reaches `/enrolled`.
3.  Confirm a `purchase` event appears in the dataLayer.
4.  Confirm `plan`, `value`, `currency`, and `transaction_id` contain
    the verified purchase data.
5.  Confirm the Google Ads Conversion tag fires **once** on the
    `purchase` event.
6.  Refresh `/enrolled` and make sure the implementation does not create
    a second purchase from an unverified/replayed request.

## Done when

`Successful purchase → /enrolled → verified purchase → dataLayer purchase event → GTM → Google Ads conversion`
