# WesternStreet multi-vendor Marketplace 

> Multi-vendor eCommerce platform built with the MERN stack & Redux.


<!-- toc -->

- [Features](#features)
- [Usage](#usage)
  - [Env Variables](#env-variables)
  - [Install Dependencies (frontend & backend)](#install-dependencies-frontend--backend)
  - [Run](#run)
- [Build & Deploy](#build--deploy)
  - [Seed Database](#seed-database)

* [Bug Fixes, corrections and code FAQ](#bug-fixes-corrections-and-code-faq)
  - [BUG: Warnings on ProfileScreen](#bug-warnings-on-profilescreen)
 
  - [BUG: All file types are allowed when updating product images](#bug-all-file-types-are-allowed-when-updating-product-images)
  - [BUG: Throwing error from productControllers will not give a custom error response](#bug-throwing-error-from-productcontrollers-will-not-give-a-custom-error-response)
    - [Original code](#original-code)
  - [BUG: Bad responses not handled in the frontend](#bug-bad-responses-not-handled-in-the-frontend)
    - [Example from PlaceOrderScreen.jsx](#example-from-placeorderscreenjsx)
  - [BUG: After switching users, our new user gets the previous users cart](#bug-after-switching-users-our-new-user-gets-the-previous-users-cart)
  - [BUG: Passing a string value to our `addDecimals` function](#bug-passing-a-string-value-to-our-adddecimals-function)
  - [BUG: Token and Cookie expiration not handled in frontend](#bug-token-and-cookie-expiration-not-handled-in-frontend)
  - [BUG: Calculation of prices as decimals gives odd results](#bug-calculation-of-prices-as-decimals-gives-odd-results)
  - [FAQ: How do I use Vite instead of CRA?](#faq-how-do-i-use-vite-instead-of-cra)
    - [Setting up the proxy](#setting-up-the-proxy)
    - [Setting up linting](#setting-up-linting)
    - [Vite outputs the build to /dist](#vite-outputs-the-build-to-dist)
    - [Vite has a different script to run the dev server](#vite-has-a-different-script-to-run-the-dev-server)
    - [A final note:](#a-final-note)
  - [FIX: issues with LinkContainer](#fix-issues-with-linkcontainer)
  * [License](#license)

<!-- tocstop -->

## Features

- Full featured shopping cart
- Product reviews and ratings
- Top products carousel
- Product pagination
- Product search feature
- User profile with orders
- Admin product management
- Admin user management
- Admin Order details page
- Mark orders as delivered option
- Checkout process (shipping, payment method, etc)
- PayPal / credit card integration
- Database seeder (products & users)

## Usage

- Create a MongoDB database and obtain your `MongoDB URI` - [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register)
- Create a PayPal account and obtain your `Client ID` - [PayPal Developer](https://developer.paypal.com/)

### Env Variables

Rename the `.env.example` file to `.env` and add the following

```
NODE_ENV = development
PORT = 5000
MONGO_URI = your mongodb uri
JWT_SECRET = 'abc123'
PAYPAL_CLIENT_ID = your paypal client id
PAGINATION_LIMIT = 8
```

Change the JWT_SECRET and PAGINATION_LIMIT to what you want

### Install Dependencies (frontend & backend)

```
npm install
cd frontend
npm install
```

### Run

```

# Run frontend (:3000) & backend (:5001)
npm run dev

# Run backend only
npm run server
```

## Build & Deploy

```
# Create frontend prod build
cd frontend
npm run build
```

### Seed Database

You can use the following commands to seed the database with some sample users and products as well as destroy all data

```
# Import data
npm run data:import

# Destroy data
npm run data:destroy
```

```
Sample User Logins

admin@email.com (Admin)
123456

john@email.com (Customer)
123456

jane@email.com (Customer)
123456
```

---



#### A final note:

Vite requires you to name React component files using the `.jsx` file
type, so you won't be able to use `.js` for your components. The entry point to
your app will be in `main.jsx` instead of `index.js`



### FIX: issues with LinkContainer

The `LinkContainer` component from [react-router-bootstrap](https://github.com/react-bootstrap/react-router-bootstrap) was used to wrap React Routers `Link` component for convenient integration between React Router and styling with Bootstrap.  
However **react-router-bootstrap** hasn't kept up with React and you may see
warnings in your console along the lines of:

```
 LinkContainer: Support for defaultProps will be removed from function components in a future major release. Use JavaScript default parameters instead.
```

Which is because React is removing default component props in favour of using
default function parameters and `LinkContainer` still uses
`Component.defaultProps`.  
However you don't really need `LinkContainer` as we can simply use the `as` prop
on any React Bootstrap component to render any element of your choice, including
React Routers `Link` component.

For example in our [Header.jsx](frontend/src/components/Header.jsx) we can first
import `Link`:

```jsx
import { useNavigate, Link } from 'react-router-dom';
```

Then instead of using `LinkContainer`:

```jsx
<LinkContainer to='/'>
  <Navbar.Brand>
    <img src={logo} alt='WesternStreet' />
    WesternStreet
  </Navbar.Brand>
</LinkContainer>
```

We can remove `LinkContainer` and use the **as** prop on the `Navbar.Brand`

```jsx
<Navbar.Brand as={Link} to='/'>
  <img src={logo} alt='WesternStreet' />
  WesternStreet
</Navbar.Brand>
```

> **Changes can be seen in:**
>
> - [Header.jsx](frontend/src/components/Header.jsx)
> - [CheckoutSteps.jsx](frontend/src/components/CheckoutSteps.jsx)
> - [Paginate.jsx](frontend/src/components/Paginate.jsx)
> - [ProfileScreen.jsx](frontend/src/screens/ProfileScreen.jsx)
> - [OrderListScreen.jsx](frontend/src/screens/admin/OrderListScreen.jsx)
> - [ProductListScreen.jsx](frontend/src/screens/admin/ProductListScreen.jsx)
> - [UserListScreen.jsx](frontend/src/screens/admin/UserListScreen.jsx)

After these changes you can then remove **react-router-bootstrap** from your
dependencies in [frontend/package.json](frontend/package.json)

Thankyou
---

