=========================== frontend ==========================

npm create vite@latest my-project -- --template react

cd my-project

npm install -D tailwindcss postcss autoprefixer

npx tailwindcss init -p

in tailwind.config file---------------
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],

in index.css file---------------------
@tailwind base;
@tailwind components;
@tailwind utilities;

npm i -D daisyui@latest
plugins: [require("daisyui")],

npm install react-router-dom

npm install sweetalert2
import Swal from 'sweetalert2'

Swal.fire({
  title: "Welcome",
  text: "successfully logged in",
  icon: "success"
});

or,
npm install sweetalert --save
import swal from 'sweetalert';
swal("Good job!", "You clicked the button!", "success");


npm install react-icons --save
npm install firebase

npm run dev
npm run build 




--------if problem occurs in hosting website in firebase, run the following commands by running cmd as administrattor----------

Get-ExecutionPolicy -List
Set-ExecutionPolicy Unrestricted
Set-ExecutionPolicy Unrestricted -Force (if the above command don't work)
Set-ExecutionPolicy Restricted






=========================== backend ==========================
-----------------firebase setup-----------------------

in config file add the folloing code -----------------

import { getAuth } from "firebase/auth";

//firebase keys are here 

const auth = getAuth(app);
export default auth;

Context API setup  (AuthProvider.jsx)---------------------------


import React, { createContext, useEffect, useState } from 'react'
import { GoogleAuthProvider, createUserWithEmailAndPassword, onAuthStateChanged, signInWithEmailAndPassword, signInWithPopup, signOut } from "firebase/auth";
import auth from './firebase.config'; // !!!!!! path should be changed according to yours


export const AuthContext = createContext();
const providerGl = new GoogleAuthProvider();

const AuthProvider = ({ children }) => {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);

    const createUser = (email, password) => {
        setLoading(true);
        return createUserWithEmailAndPassword(auth, email, password);
    }

    const createUserGoogle = () => {
        setLoading(true);
        return signInWithPopup(auth, providerGl)
    }

    const logIn = (email, password) => {
        setLoading(true);
        return signInWithEmailAndPassword(auth, email, password);
    }

    const logOut = () => {
        setLoading(true);
        return signOut(auth);
    }

    useEffect(() => {
        const unSubscribe = onAuthStateChanged(auth, currentUser => {
            setUser(currentUser);
            setLoading(false);
        });
        return () => {
            unSubscribe()
        }
    },
        [])

    const authInfo = {
        user,
        loading,
        createUser,
        logIn,
        logOut,
        createUserGoogle
    }

    return (
        <AuthContext.Provider value={authInfo}>
            {children}
        </AuthContext.Provider>
    )
}

export default AuthProvider


set up backend files----------------------------------

npm init -y
npm i express cors mongodb dotenv

"start" : "node index.js"  (in package.json file)
------------------------------------------------------

----------index.js file basic set up------------------

const express = require('express');
const cors = require('cors');
const app = express();
const port = process.env.PORT || 5000;

//middleware
app.use(cors());
app.use(express.json());

app.get("/", (req, res) => {
    res.send("Setup is ok")
})

//crud operation's code should be here

app.listen(port, () => {
    console.log(`the app is running on port ${port}`);
})
------------------------------------------------------

to run the server-----------------
nodemon index.js
or
node index.js
or
np5m start



//vercel.json file-------------------------
{
    "version": 2,
    "builds": [
        {
            "src": "index.js",
            "use": "@vercel/node"
        }
    ],
    "routes": [
        {
            "src": "/(.*)",
            "dest": "index.js",
            "methods": ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]
        }
    ]
}

//-----------------------------------------



