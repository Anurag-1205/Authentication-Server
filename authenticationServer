const express = require("express");
const fs = require("fs");
const bodyParser = require("body-parser");
const app = express();
const port = 3000;

app.use(bodyParser.json());

app.post("/login", (req, res) => {
    var userFound = null;
    fs.readFile("authentication.json", "utf8", (err, data) => {
        if(err)throw err;
        const authenticationData = JSON.parse(data);
        for(var i=0; i<authenticationData.length; i++){
            if(authenticationData[i].email === req.body.email && authenticationData[i].password === req.body.password){
                userFound = authenticationData[i];
                break;
            }
        }
        if(userFound){
            res.json({
                firstName: userFound.firstName,
                lastName: userFound.lastName,
                email: userFound.email
            });
        } else res.status(401).send();
    })
})

app.post("/signup", (req, res) => {

    let userObj = {
        email : req.body.email,
        password : req.body.password,
        firstName : req.body.firstName,
        lastName : req.body.lastName
    };
    fs.readFile("authentication.json", "utf8", (err, data) => {
        if(err)throw err;
        const authenticationData = JSON.parse(data);
        var checkUser = authenticationData.find(t => t.email === req.body.email);
        if(checkUser){
            res.status(400).send();
        }else{
            authenticationData.push(userObj);
            fs.writeFile("authentication.json", JSON.stringify(authenticationData), (err)=>{
                if(err) throw err;
                res.json();
            })
        }
    })
})

app.get("/data", (req, res) => {
    var email = req.headers.email;
    var password = req.headers.password;
    let userFound = false;

    fs.readFile("authentication.json", "utf8", (err, data) => {
        if(err)throw err;
        const authenticationData = JSON.parse(data);
        for(var i=0; i<authenticationData.length; i++){
            if (authenticationData[i].email === email && authenticationData[i].password === password) {
                userFound = true;
                break;
            }
        }
        if (userFound) {
            let usersToReturn = [];
            for (let i = 0; i<authenticationData.length; i++) {
                usersToReturn.push({
                    firstName: authenticationData[i].firstName,
                    lastName: authenticationData[i].lastName,
                    email: authenticationData[i].email
                });
            }
            res.json({
                usersToReturn
            });
        }else {
            res.sendStatus(401);
        }
    });
})

app.listen(port, () => {
    console.log(`Server in running on port: ${port}`);
})
