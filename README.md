# gatsby-plugin-netlify-identity

Gatsby plugin which adds a React Netlify Identity Widget Provider for you!

This is a very light wrapper just to help automate the boring parts, please see https://github.com/sw-yx/react-netlify-identity-widget for more info on how to use RNIW.

## Install

```bash
npm install --save gatsby-plugin-netlify-identity
## you may also need RNIW's peer dependencies
npm install --save react-netlify-identity-widget @reach/dialog @reach/tabs @reach/visually-hidden
```

## How to use

```javascript
// In your gatsby-config.js
module.exports = {
  plugins: [
    // You can should only have one instance of this plugin
    {
      resolve: `gatsby-plugin-netlify-identity`,
      options: {
        url: `https://your-identity-instance-here.netlify.app/` // required!
      }
    }
  ]
}
```

## Options

To use this, you must have a Netlify site instance with Netlify Identity enabled ([Deploy this demo to get started with one click](https://app.netlify.com/start/deploy?repository=https://github.com/sw-yx/jamstack-hackathon-starter&stack=cms&utm_source=github&utm_medium=swyx-RNI&utm_campaign=devex)). Then take the URL of that Netlify site and pass it in as the `url` for this plugin's options.

## Usage

Once installed, you are free to use `react-netlify-identity-widget`'s handy `useIdentityContext` hook anywhere in your Gatsby app. If you also want the authentication modal, you can import it as well. A good place to put this is in a Layout component, but really you could also put it in a Navbar component, it really doesn't matter since you're inside the context anyway.

Some sample code for your convenience:

```js
// src/components/Layout.js
import React from "react"
import IdentityModal, { useIdentityContext } from "react-netlify-identity-widget"
import "react-netlify-identity-widget/styles.css" // delete if you want to bring your own CSS

const Layout = ({ children }) => {
  const identity = useIdentityContext() // see https://github.com/sw-yx/react-netlify-identity for api of this identity object
  const [dialog, setDialog] = React.useState(false)
  const name =
    (identity && identity.user && identity.user.user_metadata && identity.user.user_metadata.name) || "NoName"

  console.log(JSON.stringify(identity))
  const isLoggedIn = identity && identity.isLoggedIn
  return (
    <>
      <nav style={{ background: "green" }}>
        {" "}
        Login Status:
        <button className="btn" onClick={() => setDialog(true)}>
          {isLoggedIn ? `Hello ${name}, Log out here!` : "LOG IN"}
        </button>
      </nav>
      <main>{children}</main>
      <IdentityModal showDialog={dialog} onCloseDialog={() => setDialog(false)} />
    </>
  )
}

export default Layout
```

A very simple and unmaintained demo is here: https://github.com/sw-yx/plsdelete-netlify-identity-loop-bug
