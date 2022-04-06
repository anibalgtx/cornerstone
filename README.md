Preview Code: o930ran5hf
URL: https://demo-trial.mybigcommerce.com/

1. To make the product show a second image on hover. I used this script to change the image source using javascript.

```javascript
const image = document.getElementsByClassName("card-image");
const hoverimage = image[0].getAttribute("data-hoverimage");
const dataSrc = image[0].getAttribute("data-src");

function onMouseHover() {
  image[0].setAttribute("src", hoverimage);
}
function onMouseOut() {
  image[0].setAttribute("src", dataSrc);
}
```

2. I Added an Add All Items to Cart button on the category page. It makes a request to storefront API on click.:

```javascript
    AddAllToCart() {
        $('#addToCart').on('click', () => {
            fetch('/api/storefront/carts', {
                method: 'POST',
                credentials: 'same-origin',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    lineItems: [
                        {
                            quantity: 1,
                            productId: this.context.category.products[0].id,
                        },
                    ],
                }),
            })
                .then((response) => {
                    if (response.ok) {
                        alert('Cart has been updated');
                        utils.api.cart.update();
                        window.location.reload();
                    }
                }).catch(error => console.error(error));
        });
    }
```

3. Using handlebars, I created a conditional Remove All Items button that only appears when there are items in the cart. It removes all items from de cart when clicking it.

```html
{{#if cart_id}}
<button class="button button--primary" type="button" id="removeFromCart">
  Remove All Items
</button>
{{/if}}
```

```javascript
    RemoveAllFromCart() {
        $('#removeFromCart').on('click', () => {
            fetch(`/api/storefront/carts/${this.context.cartId}`, {
                method: 'DELETE',
                credentials: 'same-origin',
                headers: {
                    'Content-Type': 'application/json',
                },
            })
                .then((response) => {
                    if (response.ok) {
                        alert('Cart has been removed');
                        utils.api.cart.update();
                        window.location.reload();
                    }
                }).catch(error => console.error(error));
        });
    }
```

4. Using front matters & inject. I have access to the customer data. When a user is logged in. I show a banner with the user's email, name & phone.

```html
{{#if customer.email}}
<p>{{customer.email}}</p>
{{/if}} {{#if customer.name}}
<p>{{customer.name}}</p>
{{/if}} {{#if customer.phone}}
<p>{{phoneNumber customer.phone}}</p>
{{/if}}
```
