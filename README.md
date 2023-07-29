# MetaMask Feedback Portal

## Overview

This project serves as a practical demonstration of how the integration of MetaMask with a simple web application and smart contract deployment on the Ethereum blockchain can enable the creation of a decentralized feedback system. Users can securely submit feedback, query feedback data, and interact with the dApp without the need for a central authority, showcasing the power and potential of blockchain technology in building decentralized and trustless applications.

## Getting Start

To build this project we have to follow the following steps:

To begin, we must install Ganache, which will serve as a network for my metamask wallet. Following that, we must frequently check to see if the web server is properly running in the system. This can be avoided by typing "node server.js" into the cmd navigated by the project's directory. In addition, we must open a web browser and navigate to http://localhost:2811 or any other specified port number in order to access the web application.

## Description

### Server.js file

	//Express framework in JavaScript
	const express = require("express");
	const path = require("path");
	const app = express();
	
	app.get("/", (req, res) => {
	    res.sendFile(path.join(__dirname + "/index.html"));
	})
	
	const server = app.listen(process.env.PORT || 2811);
	const portNumber = server.address().port;
	console.log(`port is running on ${portNumber}`);

This project shows how to use the Express framework to create a simple web server in JavaScript. Express is a popular Node.js framework that makes it easier to create web applications and APIs. In the project directory, run the following command to start the Express server: 'npm start' is a command. The server will listen on a specified port (in this case, port 2811) and print the port number in the console. This project's Express server defines a single route that handles requests to the root URL ("/"). When a client sends a GET request to the root URL, the server returns the index.html file from the project directory. To change the server's behaviour or add new routes, you can change the app.get() function. When a request is made to the specified route, the callback function within app.get() is executed. Within this callback function, you can define the desired response logic. 


### Smart.sol

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    contract MetaMaskInfo {
    address public owner;

    struct Feedback {
        uint8 rating; // Rating from 1 to 5
        string comment;
    }

    mapping(address => Feedback[]) public userFeedbacks;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }


    function submitFeedback(uint8 _rating, string memory _comment) public {
        require(_rating >= 1 && _rating <= 5, "Invalid rating, please choose between 1 and 5");
        require(bytes(_comment).length > 0, "Comment cannot be empty");

        userFeedbacks[msg.sender].push(Feedback(_rating, _comment));
    }

    function getUserFeedbackCount(address _user) public view returns (uint256) {
        return userFeedbacks[_user].length;
    }

    function getUserFeedback(address _user, uint256 _index) public view returns (uint8, string memory) {
        require(_index < userFeedbacks[_user].length, "Invalid feedback index");
        Feedback memory feedback = userFeedbacks[_user][_index];
        return (feedback.rating, feedback.comment);
    }
    }

The provided Solidity smart contract is named "MetaMaskInfo," and it serves as a basic feedback system on the Ethereum blockchain. The contract allows users to submit feedback with a rating (ranging from 1 to 5) and a comment. This feedback is stored on-chain using a mapping that associates user addresses with arrays of "Feedback" structures.

Key components of the smart contract:

1. State Variables:
   - `address public owner`: Stores the address of the contract owner, who has special privileges due to the `onlyOwner` modifier.

2. Struct:
   - `struct Feedback`: Defines the structure to hold feedback data, consisting of a uint8 rating and a string comment.

3. Mapping:
   - `mapping(address => Feedback[]) public userFeedbacks`: Maps user addresses to arrays of feedback data. Each user can have multiple feedback entries stored in an array.

4. Constructor:
   - The constructor sets the contract deployer's address as the `owner`.

5. Modifiers:
   - `modifier onlyOwner()`: Restricts access to certain functions, allowing only the contract owner to call them.

6. Functions:
   - `submitFeedback`: Allows users to submit their feedback with a valid rating (1 to 5) and a non-empty comment. The feedback is added to the `userFeedbacks` mapping under the sender's address.
   - `getUserFeedbackCount`: Enables users to retrieve the total number of feedback entries they have submitted.
   - `getUserFeedback`: Allows users to retrieve specific feedback data based on the user's address and the index of the feedback entry in their array.

The contract's purpose is to provide a simple and decentralized way for users to provide feedback, which is then permanently recorded on the Ethereum blockchain. By leveraging blockchain technology, the contract ensures data transparency, immutability, and trustlessness, as no centralized authority can modify or delete the feedback entries once they are recorded. Users can interact with this feedback system using a blockchain wallet like MetaMask and query the contract to retrieve their feedback data.

### index.html

    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8" />
    <title>LINK TO METAMASK</title>
    <script
      type="text/javascript"
      src="https://cdn.jsdelivr.net/npm/web3@1.3.5/dist/web3.min.js"
    ></script>
    <style>
      body {
        background-image: linear-gradient(
            45deg,
            rgba(13, 0, 61, 0.2) 0%,
            rgba(13, 0, 61, 0.2) 16.667%,
            rgba(14, 79, 102, 0.2) 16.667%,
            rgba(14, 79, 102, 0.2) 33.334%,
            rgba(15, 158, 143, 0.2) 33.334%,
            rgba(15, 158, 143, 0.2) 50.001%,
            rgba(16, 198, 163, 0.2) 50.001%,
            rgba(16, 198, 163, 0.2) 66.668%,
            rgba(15, 119, 122, 0.2) 66.668%,
            rgba(15, 119, 122, 0.2) 83.335%,
            rgba(14, 40, 81, 0.2) 83.335%,
            rgba(14, 40, 81, 0.2) 100.002%
          ),
          linear-gradient(
            22.5deg,
            rgba(13, 0, 61, 0.2) 0%,
            rgba(13, 0, 61, 0.2) 16.667%,
            rgba(14, 79, 102, 0.2) 16.667%,
            rgba(14, 79, 102, 0.2) 33.334%,
            rgba(15, 158, 143, 0.2) 33.334%,
            rgba(15, 158, 143, 0.2) 50.001%,
            rgba(16, 198, 163, 0.2) 50.001%,
            rgba(16, 198, 163, 0.2) 66.668%,
            rgba(15, 119, 122, 0.2) 66.668%,
            rgba(15, 119, 122, 0.2) 83.335%,
            rgba(14, 40, 81, 0.2) 83.335%,
            rgba(14, 40, 81, 0.2) 100.002%
          ),
          linear-gradient(
            0deg,
            rgba(13, 0, 61, 0.2) 0%,
            rgba(13, 0, 61, 0.2) 16.667%,
            rgba(14, 79, 102, 0.2) 16.667%,
            rgba(14, 79, 102, 0.2) 33.334%,
            rgba(15, 158, 143, 0.2) 33.334%,
            rgba(15, 158, 143, 0.2) 50.001%,
            rgba(16, 198, 163, 0.2) 50.001%,
            rgba(16, 198, 163, 0.2) 66.668%,
            rgba(15, 119, 122, 0.2) 66.668%,
            rgba(15, 119, 122, 0.2) 83.335%,
            rgba(14, 40, 81, 0.2) 83.335%,
            rgba(14, 40, 81, 0.2) 100.002%
          ),
          linear-gradient(90deg, rgb(73, 73, 73), rgb(94, 94, 94));
        height: 100vh;
        font-size: 21px;
        text-align: center;
      }
      .button {
        appearance: none;
        background-color: #ffffff;
        border-radius: 40em;
        border-style: none;
        box-shadow: #adcfff 0 -12px 6px inset;
        box-sizing: border-box;
        color: #000000;
        cursor: pointer;
        display: inline-block;
        font-family: -apple-system, sans-serif;
        font-size: 1.2rem;
        font-weight: 700;
        letter-spacing: -0.24px;
        margin: 0;
        outline: none;
        padding: 1rem 1.3rem;
        quotes: auto;
        text-align: center;
        text-decoration: none;
        transition: all 0.15s;
        -webkit-user-select: none;
        user-select: none;
        touch-action: manipulation;
      }
      .button:hover {
        background-color: #ffc229;
        box-shadow: #b41908 0 -6px 8px inset;
        transform: scale(1.125);
      }
      .button:active {
        transform: scale(1.025);
      }
      @media (min-width: 768px) {
        .button {
          font-size: 1.5rem;
          padding: 0.75rem 2rem;
        }
      }
    </style>
    </head>
    <body>
    <button class="button" onclick="connectMetamask()">METAMASK LINKAGE</button>
    <br />
    <p id="accountArea"></p>
    <button class="button" onclick="connectContract()">
      SMART CONTRACT CONNECT
    </button>
    <br />
    <p id="contractArea"></p>
    <button class="button" onclick="submitFeedback(5, 'Great job!')">
      Submit Feedback
    </button>
    <br />
    <p id="feedbackArea"></p>
    <button class="button" onclick="getUserFeedbackCount()">
      Get User Feedback Count
    </button>
    <br />
    <p id="errorArea" style="color: red"></p>
    <button class="button" onclick="getUserFeedback(0)">
      Get User Feedback at Index 0
    </button>
    <br />

    <script>
      let account;

      // Metamask Linkage
      const connectMetamask = async () => {
        if (window.ethereum !== "undefined") {
          const accounts = await ethereum.request({
            method: "eth_requestAccounts",
          });
          account = accounts[0];
          document.getElementById("accountArea").innerHTML = account;
        }
      };

      // Smart contract connection
      const connectContract = async () => {
        const ABI = [
          {
            inputs: [
              {
                internalType: "uint8",
                name: "_rating",
                type: "uint8",
              },
              {
                internalType: "string",
                name: "_comment",
                type: "string",
              },
            ],
            name: "submitFeedback",
            outputs: [],
            stateMutability: "nonpayable",
            type: "function",
          },
          {
            inputs: [],
            stateMutability: "nonpayable",
            type: "constructor",
          },
          {
            inputs: [
              {
                internalType: "address",
                name: "_user",
                type: "address",
              },
              {
                internalType: "uint256",
                name: "_index",
                type: "uint256",
              },
            ],
            name: "getUserFeedback",
            outputs: [
              {
                internalType: "uint8",
                name: "",
                type: "uint8",
              },
              {
                internalType: "string",
                name: "",
                type: "string",
              },
            ],
            stateMutability: "view",
            type: "function",
          },
          {
            inputs: [
              {
                internalType: "address",
                name: "_user",
                type: "address",
              },
            ],
            name: "getUserFeedbackCount",
            outputs: [
              {
                internalType: "uint256",
                name: "",
                type: "uint256",
              },
            ],
            stateMutability: "view",
            type: "function",
          },
          {
            inputs: [],
            name: "owner",
            outputs: [
              {
                internalType: "address",
                name: "",
                type: "address",
              },
            ],
            stateMutability: "view",
            type: "function",
          },
          {
            inputs: [
              {
                internalType: "address",
                name: "",
                type: "address",
              },
              {
                internalType: "uint256",
                name: "",
                type: "uint256",
              },
            ],
            name: "userFeedbacks",
            outputs: [
              {
                internalType: "uint8",
                name: "rating",
                type: "uint8",
              },
              {
                internalType: "string",
                name: "comment",
                type: "string",
              },
            ],
            stateMutability: "view",
            type: "function",
          },
        ];

        const Address = "0xb858eB95B9777cE5faA52001Ef26e0f5aE627798";
        window.web3 = await new Web3(window.ethereum);
        contract = await new window.web3.eth.Contract(ABI, Address);
        document.getElementById("contractArea").innerHTML =
          "Connected to smart contract";
      };

      // Function to submit feedback
      async function submitFeedback() {
        try {
          const rating = parseInt(prompt("Enter rating (1-5):"));
          const comment = prompt("Enter feedback comment:");
          if (isNaN(rating) || rating < 1 || rating > 5) {
            document.getElementById("errorArea").innerHTML =
              "Invalid rating. Please choose a rating between 1 and 5.";
            return;
          }
          if (comment === "") {
            document.getElementById("errorArea").innerHTML =
              "Comment cannot be empty.";
            return;
          }

          if (!contract) {
            document.getElementById("errorArea").innerHTML =
              "Smart contract not connected.";
            return;
          }

          // Call the smart contract's "submitFeedback" function
          await contract.methods
            .submitFeedback(rating, comment)
            .send({ from: account });
          document.getElementById("errorArea").innerHTML = "";
          console.log("Feedback submitted successfully!");
        } catch (error) {
          document.getElementById("errorArea").innerHTML =
            "Error submitting feedback: " + error.message;
        }
      }

      // Function to retrieve user feedback count
      async function getUserFeedbackCount() {
        try {
          if (!contract) {
            document.getElementById("errorArea").innerHTML =
              "Smart contract not connected.";
            return;
          }

          // Call the smart contract's "getUserFeedbackCount" function
          const userFeedbackCount = await contract.methods
            .getUserFeedbackCount(account)
            .call();
          document.getElementById("feedbackArea").innerHTML =
            "User feedback count: " + userFeedbackCount;
          document.getElementById("errorArea").innerHTML = "";
          console.log("User feedback count:", userFeedbackCount);
        } catch (error) {
          document.getElementById("errorArea").innerHTML =
            "Error getting user feedback count: " + error.message;
        }
      }

      // Function to retrieve user feedback at a specific index
      async function getUserFeedback() {
        try {
          const index = parseInt(prompt("Enter feedback index:"));
          if (isNaN(index) || index < 0) {
            document.getElementById("errorArea").innerHTML =
              "Invalid index. Please enter a non-negative integer.";
            return;
          }

          if (!contract) {
            document.getElementById("errorArea").innerHTML =
              "Smart contract not connected.";
            return;
          }

          // Call the smart contract's "getUserFeedback" function
          const feedback = await contract.methods
            .getUserFeedback(account, index)
            .call();
          document.getElementById("feedbackArea").innerHTML =
            "Feedback at index " +
            index +
            ": Rating - " +
            feedback[0] +
            ", Comment - " +
            feedback[1];
          document.getElementById("errorArea").innerHTML = "";
          console.log("Feedback at index", index, ":", feedback);
        } catch (error) {
          document.getElementById("errorArea").innerHTML =
            "Error getting user feedback: " + error.message;
        }
      }
    </script>
    </body>
    </html>



The provided HTML file contains a simple web application that demonstrates the integration of MetaMask and interaction with a smart contract deployed on the Ethereum blockchain. The application allows users to connect their MetaMask wallet, interact with the smart contract, and perform the following actions:

1. **Connect MetaMask**: Clicking the "METAMASK LINKAGE" button will prompt the user to connect their MetaMask wallet to the application. If the user grants permission, their Ethereum address will be displayed in the "accountArea" section.

2. **Connect Smart Contract**: Clicking the "SMART CONTRACT CONNECT" button will connect the web application to a specific smart contract deployed at the address `0xb858eB95B9777cE5faA52001Ef26e0f5aE627798` on the Ethereum blockchain. If successful, the message "Connected to smart contract" will be displayed in the "contractArea" section.

3. **Submit Feedback**: Clicking the "Submit Feedback" button will prompt the user to enter a rating (between 1 and 5) and a feedback comment. If valid data is provided and the smart contract is connected, the feedback will be submitted to the contract using the "submitFeedback" function. Any error or success messages will be displayed in the "errorArea" section.

4. **Get User Feedback Count**: Clicking the "Get User Feedback Count" button will retrieve the total number of feedback entries submitted by the connected MetaMask account. The count will be displayed in the "feedbackArea" section.

5. **Get User Feedback at Index 0**: Clicking the "Get User Feedback at Index 0" button will prompt the user to enter an index number. The application will then fetch the feedback data at that index from the smart contract using the "getUserFeedback" function and display it in the "feedbackArea" section.

Please note that to use this web application, users must have the MetaMask extension installed in their web browser and be connected to the Ethereum Mainnet or a compatible testnet. Additionally, the contract address (`0xb858eB95B9777cE5faA52001Ef26e0f5aE627798`) in the JavaScript code should correspond to the deployed smart contract that supports the functions mentioned in the code.

The application provides a user-friendly interface for interacting with a blockchain-based feedback system and showcases how MetaMask can be used to interact with Ethereum smart contracts from a web browser.
## Lincense 

This project is licensed under the MIT License.












 
