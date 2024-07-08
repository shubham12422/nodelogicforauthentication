# nodelogicforauthentication
const jwt = require ("jsonwebtoken");

require("dotenv").config();

const User= require("../models/User");




// auth
// This code defines an auth middleware function for a Node.js application, typically using the Express framework. This middleware function is used to authenticate users based on a JSON Web Token (JWT). Here's a step-by-step explanation of what each part of the code does:


exports.auth = async ( req, res , next)  => {
    try{



        const token = req.cookies.token
                        || req.body.token
                        || req.header ("Authorization").replace("Bearer","");

        if (!token) {

            return res.status(401).json({
                success:false,
                message:"token is missing",
            })
        }


        try{
            const decode = await jwt.verify(token , process.env.JWT_SECRET);
            console.log(decode);
            req.user=decode;
        }catch(err){
            // verification error:
            return res.status(401).json({
                success:false,
                message:"token is invalid",
            });

        }
        next();
    }
    catch(error){
        return res.status(401).json({
            success:false,
            message:"something went wrong while validating the token"
        })
    }
}










// isStudent
exports.isStudent =  async (req,res, next) => {

    try{

        if (req.user.accountType !== "student"){


            return req.status(401).json({
                success:false,
                message:"this is  a protected route for students only",
            });
        }

        next();
            

           
    }catch(error){


        return res.status(500).json({
            sucess:false,
            message:"user role cannot be verified, plz try again"
        })
    }

     

}



// isInstructer
exports.isInstructor =  async (req,res, next) => {

    try{

        if (req.user.accountType !== "Instructer"){


            return req.status(401).json({
                success:false,
                message:"this is  a protected route for instructer",
            });
        }

        next();
            

           
    }catch(error){


        return res.status(500).json({
            sucess:false,
            message:"user role cannot be verified, plz try again"
        })
    }

     

}


// isAdmin

xports.isAdmin =  async (req,res, next) => {

    try{

        if (req.user.accountType !== "student"){


            return req.status(401).json({
                success:false,
                message:"this is  a protected route for admin only",
            });
        }

        next();
            

           
    }catch(error){


        return res.status(500).json({
            sucess:false,
            message:"user role cannot be verified, plz try again"
        })
    }

     

}





