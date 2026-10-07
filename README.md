# Google Tag Manager Installation --- Ivy Tutors Network

Please install the following Google Tag Manager container on
**ivytutorsnetwork.com**.

**GTM Container ID:** `GTM-MCKF7VXW`

The container should be installed site-wide so it is available on all
pages, including:

`https://ivytutorsnetwork.com/enrolled`

## 1. Add to `<head>`

Paste this code as high as possible inside the `<head>` of every page:

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

## 2. Add after `<body>`

Paste this code immediately after the opening `<body>` tag:

``` html
<!-- Google Tag Manager (noscript) -->
<noscript>
<iframe
src="https://www.googletagmanager.com/ns.html?id=GTM-MCKF7VXW"
height="0"
width="0"
style="display:none;visibility:hidden">
</iframe>
</noscript>
<!-- End Google Tag Manager (noscript) -->
```

## Done

Once deployed, please confirm that **GTM-MCKF7VXW** is installed and
loading on:

`https://ivytutorsnetwork.com/enrolled`

No additional tracking or Google Ads configuration is needed from you.
