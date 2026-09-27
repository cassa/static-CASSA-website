---
title: Store
description: Cool CASSA Clothes™
icon: fa-solid fa-cart-shopping
type: docs
outputs: [HTML, markdown]

sidebar_root_for: self
sidebar_root_link_self: true
menu:
    main:
        identifier: store
        weight: 40 # order in the bar (lower the earlier)
---

![Image of Two Models doing spiderman meme with CASSA `StorePage` in the background](1.png)

<!-- Sorry to Anyone reading this.
    indentiation is not allowed or hugo thinks the code should be rendered on page

    Also I stitched this together from the minimal documentation
-->

<script src="https://cdn.shopify.com/storefront/web-components.js"></script>

<shopify-store
    id="store"
  store-domain="https://cassa-au.myshopify.com">
</shopify-store>

<script>
  function showProductDetails(event) {
    // Update a dialog context with a selected product
    document.getElementById('dialog-context')
      .update(event);

    // Show the dialog
    document.getElementById('dialog')
      .showModal();
  }
</script>

<style>
    .product-card {
        background-color: var(--td-brand-elev);
        padding: clamp(22px, 3vw, 30px);
        margin: 2.25%;
        width: 45%;
        border-radius: 12px;
        border: 1px solid var(--bs-border-color);
    }

    .product-card shopify-media img {
        margin: 0 0 10px;
        border-radius: 12px;
    }

    dialog {
        background-color: var(--td-brand-elev);
        padding: clamp(22px, 3vw, 30px);
        width: 45%;
        border-radius: 12px;
        border: 1px solid var(--bs-border-color);
    }

    .product-popup .product-card-image img {
        border-radius: 12px;
        width: 80% !important;
        display: block;
        margin: auto;
    }

    .product-popup .buy-button {
        border-radius: 8px;
        min-height: 44px;
        color: #fff;
        background: #2f6793;
        box-shadow: 0 6px 22px rgba(47, 103, 147, 0.3);
        padding: 12px 24px;
        font-size: 0.98rem;
        font-weight: 700;
        transition: transform 160ms ease, box-shadow 160ms ease, background-color 160ms ease, border-color 160ms ease, color 160ms ease;
        width: 100%;
        border: 1px solid transparent;
    }

    .product-popup .buy-button:hover {
        color: #fff;
        background: #275a82;
        box-shadow: 0 10px 30px rgba(47, 103, 147, 0.38);
        transform: translateY(-2px);
    }


    /* Slideshow stuff */
    .prev, .next {
        cursor: pointer;
        position: absolute;
        top: 35%;
        width: auto;
        padding: 16px;
        margin-top: -22px;
        color: var(--td-brand-silk);
        font-weight: bold;
        font-size: 18px;
        transition: 0.6s ease;
        border-radius: 0 3px 3px 0;
        user-select: none;
    }

    .next {
        right: 0;
        border-radius: 3px 0 0 3px;
    }

    .prev:hover, .next:hover {
        background-color: var(--td-brand-header-bg);
    }

    /* Fading animation */
    .fade {
        animation-name: fade;
        animation-duration: 1.5s;
    }

    @keyframes fade {
        from {opacity: .4} 
        to {opacity: 1}
    }
</style>

<shopify-list-context
  type="product"
  query="products"
  first="20">
<template>
<button onclick="showProductDetails(event)" class="product-card">
    <shopify-media 
        max-images="1"
        query="product.featuredImage"
    ></shopify-media>
    <h3><shopify-data query="product.title"></shopify-data></h3>
    $<shopify-money query="product.selectedOrFirstAvailableVariant.price" format="money_without_currency"></shopify-money>
</button>
</template>
</shopify-context>


<dialog id="dialog">
<!-- A product context that waits for an update to render -->
<shopify-context
    id="dialog-context"
    type="product"
    wait-for-update>
<template>
<div class="product-popup">
<button id="close-button" onclick="this.parentNode.parentNode.parentNode.close()" style="float: right; background: transparent; border: 0px;">X</button>

<shopify-list-context
    type="image"
    query="product.selectedOrFirstAvailableVariant.product.images"
    first="5">
<template>

<div class="product-card-image">
<shopify-media class="product-card-image-large" query="image"></shopify-media>
</div>

<script>
showSlides(slideIndex);
</script>

</template>

</shopify-list-context>
<a class="prev" onclick="plusSlides(-1)">❮</a>
<a class="next" onclick="plusSlides(1)">❯</a>
<br>
<shopify-data query="product.description"></shopify-data>
<shopify-variant-selector
    visible-option="size">
</shopify-variant-selector>
<br>
<button
    onclick="document.querySelector('shopify-store').buyNow(event, {target: '_blank'});"
    shopify-attr--disabled="!product.selectedOrFirstAvailableVariant.availableForSale"
    class="buy-button">
    <i class="fa-solid fa-shopping-cart" aria-hidden="true"></i> Buy
</button>

</div>

<script>
let slideIndex = 1;

function plusSlides(n) {
    showSlides(slideIndex += n);
}

function currentSlide(n) {
    showSlides(slideIndex = n);
}

function showSlides(n) {
    let i;
    let slides = document.getElementsByClassName("product-card-image");
    if (n > slides.length) {slideIndex = 1}    
    if (n < 1) {slideIndex = slides.length}
    for (i = 0; i < slides.length; i++) {
        slides[i].style.display = "none";  
    }
    slides[slideIndex-1].style.display = "block";
}
</script>

</template>

</shopify-context>
</dialog>