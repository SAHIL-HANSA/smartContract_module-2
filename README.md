# MetaMask Web Application with Express and Solidity Smart Contract

## Overview

 This project demonstrates the integration of MetaMask, a popular cryptocurrency wallet and decentralized application platform, with a custom Solidity smart contract. The user can interact with the smart contract and display various information on the web page using the provided buttons. It serves as a basic example of how to interact with Ethereum blockchain data and smart contracts using JavaScript and Web3.js within a web application.

## Getting Start

To build this project we have to follow the following steps:

To begin, we must install Ganache, which will serve as a network for my metamask wallet. Following that, we must frequently check to see if the web server is properly running in the system. This can be avoided by typing "node server.js" into the cmd navigated by the project's directory. In addition, we must open a web browser and navigate to http://localhost:2811 or any other specified port number in order to access the web application.

## Description

### Server.js

	//Express framework in JavaScript
	const express = require("express");
	const path = require("path");
	const app = express();
	
	app.get("/", (req, res) => {
	    res.sendFile(path.join(__dirname + "/index.html"));
	})
	
	const server = app.listen(1725);
	const portNumber = server.address().port;
	console.log(`port is running on ${portNumber}`);

This project shows how to use the Express framework to create a simple web server in JavaScript. Express is a popular Node.js framework that makes it easier to create web applications and APIs. In the project directory, run the following command to start the Express server: 'npm start' is a command. The server will listen on a specified port (in this case, port 2811) and print the port number in the console. This project's Express server defines a single route that handles requests to the root URL ("/"). When a client sends a GET request to the root URL, the server returns the index.html file from the project directory. To change the server's behaviour or add new routes, you can change the app.get() function. When a request is made to the specified route, the callback function within app.get() is executed. Within this callback function, you can define the desired response logic. 


### Simple.sol

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    contract MetaMaskInfo {
    address public owner;
    uint256 public numberOfSites;
    string public connectedNetwork;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function getNumberOfSites() public view returns (uint256) {
        return numberOfSites;
    }

    function getConnectedNetwork() public view returns (string memory) {
        return connectedNetwork;
    }

    // Function to set the number of sites connected to MetaMask wallet
    function setNumberOfSites(uint256 _sites) public onlyOwner {
        numberOfSites = _sites;
    }

    // Function to set the name of the network connected to MetaMask wallet
    function setConnectedNetwork(string memory _network) public onlyOwner {
        connectedNetwork = _network;
     }
    }


This Solidity smart contract is designed to store and manage information related to a user's MetaMask wallet. It includes the following functionalities:

1. Contract Variables:
   - "owner": An Ethereum address representing the owner of the smart contract. It is set during contract deployment to the address of the account deploying the contract.
   - "numberOfSites": An unsigned integer variable that stores the number of sites connected to the user's MetaMask wallet.
   - "connectedNetwork": A string variable that stores the name of the network connected to the user's MetaMask wallet.

2. Constructor:
   - The contract constructor initializes the "owner" variable with the address of the account that deploys the contract.

3. Modifier:
   - The "onlyOwner" modifier restricts access to certain functions so that only the contract owner (the account that deployed the contract) can call them. If other accounts attempt to call these functions, a "require" statement will prevent their execution.

4. Public Functions:
   - "getNumberOfSites()": A view function that allows anyone to read (view) the value of the "numberOfSites" variable.
   - "getConnectedNetwork()": A view function that allows anyone to read (view) the value of the "connectedNetwork" variable.

5. Setter Functions:
   - "setNumberOfSites(uint256 _sites)": A function that allows the contract owner to set the number of sites connected to the MetaMask wallet by providing a new value for the "numberOfSites" variable.
   - "setConnectedNetwork(string memory _network)": A function that allows the contract owner to set the name of the network connected to the MetaMask wallet by providing a new value for the "connectedNetwork" variable.

The contract provides basic functionalities for storing and retrieving information related to the user's MetaMask wallet, with certain operations restricted to the contract owner. It could be integrated with a front-end application, such as the one shown earlier, to interact with the contract and display the stored information on a user interface.


### index.html

	<!DOCTYPE html>
    <html>
    <head>
    <title>LINK TO METAMASK</title>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/web3/1.2.7-rc.0/web3.min.js"></script>
    <style>
        body {
            background-image: linear-gradient(45deg, rgba(13, 0, 61,0.2) 0%, rgba(13, 0, 61,0.2) 16.667%,rgba(14, 79, 102,0.2) 16.667%, rgba(14, 79, 102,0.2) 33.334%,rgba(15, 158, 143,0.2) 33.334%, rgba(15, 158, 143,0.2) 50.001%,rgba(16, 198, 163,0.2) 50.001%, rgba(16, 198, 163,0.2) 66.668%,rgba(15, 119, 122,0.2) 66.668%, rgba(15, 119, 122,0.2) 83.335%,rgba(14, 40, 81,0.2) 83.335%, rgba(14, 40, 81,0.2) 100.002%),linear-gradient(22.5deg, rgba(13, 0, 61,0.2) 0%, rgba(13, 0, 61,0.2) 16.667%,rgba(14, 79, 102,0.2) 16.667%, rgba(14, 79, 102,0.2) 33.334%,rgba(15, 158, 143,0.2) 33.334%, rgba(15, 158, 143,0.2) 50.001%,rgba(16, 198, 163,0.2) 50.001%, rgba(16, 198, 163,0.2) 66.668%,rgba(15, 119, 122,0.2) 66.668%, rgba(15, 119, 122,0.2) 83.335%,rgba(14, 40, 81,0.2) 83.335%, rgba(14, 40, 81,0.2) 100.002%),linear-gradient(0deg, rgba(13, 0, 61,0.2) 0%, rgba(13, 0, 61,0.2) 16.667%,rgba(14, 79, 102,0.2) 16.667%, rgba(14, 79, 102,0.2) 33.334%,rgba(15, 158, 143,0.2) 33.334%, rgba(15, 158, 143,0.2) 50.001%,rgba(16, 198, 163,0.2) 50.001%, rgba(16, 198, 163,0.2) 66.668%,rgba(15, 119, 122,0.2) 66.668%, rgba(15, 119, 122,0.2) 83.335%,rgba(14, 40, 81,0.2) 83.335%, rgba(14, 40, 81,0.2) 100.002%),linear-gradient(90deg, rgb(73, 73, 73),rgb(94, 94, 94));
            height: 100vh;
            font-size: 21px;
            text-align: center;
        }
        .button {
        appearance: none;
        background-color: #FFFFFF;
        border-radius: 40em;
        border-style: none;
        box-shadow: #ADCFFF 0 -12px 6px inset;
        box-sizing: border-box;
        color: #000000;
        cursor: pointer;
        display: inline-block;
        font-family: -apple-system,sans-serif;
        font-size: 1.2rem;
        font-weight: 700;
        letter-spacing: -.24px;
        margin: 0;
        outline: none;
        padding: 1rem 1.3rem;
        quotes: auto;
        text-align: center;
        text-decoration: none;
        transition: all .15s;
        user-select: none;
        -webkit-user-select: none;
        touch-action: manipulation;
        }
        .button:hover {
        background-color: #FFC229;
        box-shadow: #b41908 0 -6px 8px inset;
        transform: scale(1.125);
        }
        .button:active {
        transform: scale(1.025);
        }
        @media (min-width: 768px) {
        .button {
            font-size: 1.5rem;
            padding: .75rem 2rem;
        }
        }

    </style>
    </head>
    <body>
    <button class="button" onclick="connectMetamask()">METAMASK LINKAGE</button> <br>
    <p id="accountArea"></p>
    <button class="button" onclick="connectContract()">SMART CONTRACT CONNECT</button> <br>
    <p id="contractArea"></p>
    <button class="button" onclick="trackAddress()">TRACK LAST ADDRESS</button> <br>
    <p id="addressArea"></p>
    <button class="button" onclick="getNumberOfSites()">GET CONNECT SITES</button> <br>
    <p id="getsiteArea"></p>
    <button class="button" onclick="getConnectedNetwork()">GET CONNECT NETWORK</button> <br>
    <p id="networkArea"></p>

    <script>
        let account;

        // Metamask Linkage
        const connectMetamask = async () => {
            if (window.ethereum !== "undefined") {
                const accounts = await ethereum.request({ method: "eth_requestAccounts" });
                account = accounts[0];
                document.getElementById("accountArea").innerHTML = account;
            }
        };

        // Smart contract connection
        const connectContract = async () => {
            const ABI = [
            {
                "inputs": [
                    {
                        "internalType": "string",
                        "name": "_network",
                        "type": "string"
                    }
                ],
                "name": "setConnectedNetwork",
                "outputs": [],
                "stateMutability": "nonpayable",
                "type": "function"
            },
            {
                "inputs": [
                    {
                        "internalType": "uint256",
                        "name": "_sites",
                        "type": "uint256"
                    }
                ],
                "name": "setNumberOfSites",
                "outputs": [],
                "stateMutability": "nonpayable",
                "type": "function"
            },
            {
                "inputs": [],
                "stateMutability": "nonpayable",
                "type": "constructor"
            },
            {
                "inputs": [],
                "name": "connectedNetwork",
                "outputs": [
                    {
                        "internalType": "string",
                        "name": "",
                        "type": "string"
                    }
                ],
                "stateMutability": "view",
                "type": "function"
            },
            {
                "inputs": [],
                "name": "getConnectedNetwork",
                "outputs": [
                    {
                        "internalType": "string",
                        "name": "",
                        "type": "string"
                    }
                ],
                "stateMutability": "view",
                "type": "function"
            },
            {
                "inputs": [],
                "name": "getNumberOfSites",
                "outputs": [
                    {
                        "internalType": "uint256",
                        "name": "",
                        "type": "uint256"
                    }
                ],
                "stateMutability": "view",
                "type": "function"
            },
            {
                "inputs": [],
                "name": "numberOfSites",
                "outputs": [
                    {
                        "internalType": "uint256",
                        "name": "",
                        "type": "uint256"
                    }
                ],
                "stateMutability": "view",
                "type": "function"
            },
            {
                "inputs": [],
                "name": "owner",
                "outputs": [
                    {
                        "internalType": "address",
                        "name": "",
                        "type": "address"
                    }
                ],
                "stateMutability": "view",
                "type": "function"
            }
        ];
            
            const Address = "0xD23102345b57AC3E2b612F58b5856E4d43Cf6504";
            window.web3 = await new Web3(window.ethereum);
            contract = await new window.web3.eth.Contract(ABI, Address);
            document.getElementById("contractArea").innerHTML = "Connected to smart contract";
        };


        // Track last transaction address
        const trackAddress = async () => {
            if (window.web3) {
                try {
                    const latestBlockNumber = await window.web3.eth.getBlockNumber();
                    const latestBlock = await window.web3.eth.getBlock(latestBlockNumber);
                    if (latestBlock.transactions.length > 0) {
                        const latestTransactionHash = latestBlock.transactions[0];
                        const transaction = await window.web3.eth.getTransaction(latestTransactionHash);
                        document.getElementById("addressArea").innerHTML = `Recipient Address: ${transaction.to}`;
                    } else {
                        document.getElementById("addressArea").innerHTML = "No transactions found in the latest block.";
                    }
                } catch (error) {
                    console.error("Error fetching transaction details:", error);
                }
            } else {
                console.error("Web3 is not available. Please connect to Metamask.");
            }
        };


        //Get connected Sites

        const getNumberOfSites = async () => {
            if (contract) {
                const numberOfSites = await contract.methods.getNumberOfSites().call();
                document.getElementById("getsiteArea").innerHTML = `Number of Sites: ${numberOfSites}`;
            } else {
                console.error("Smart contract is not connected. Please connect to the contract first.");
            }
        };


        // Get the network conected to metamask
        const getConnectedNetwork = async () => {
            if (contract) {
                const connectedNetwork = await contract.methods.getConnectedNetwork().call();
                document.getElementById("networkArea").innerHTML = `Connected Network: ${connectedNetwork}`;
            } else {
                console.error("Smart contract is not connected. Please connect to the contract first.");
            }
        };

    </script>
    </body>
    </html>


This HTML file demonstrates a simple web page with buttons that interact with the MetaMask wallet and a Solidity smart contract deployed on the Ethereum blockchain.

1. Metamask Linkage:
   - The first button labeled "METAMASK LINKAGE" allows users to connect their MetaMask wallet to the web application.
   - When the button is clicked, the JavaScript function "connectMetamask()" is triggered.
   - If the user's MetaMask extension is available, the function requests access to the user's accounts and displays the account address in the paragraph with the id "accountArea."

2. Smart Contract Connection:
   - The second button labeled "SMART CONTRACT CONNECT" establishes a connection with the deployed Solidity smart contract.
   - The JavaScript function "connectContract()" is invoked when the button is clicked.
   - The function interacts with the contract using the ABI (Application Binary Interface) and the contract address provided in the "Address" variable.
   - A message "Connected to smart contract" is displayed in the paragraph with the id "contractArea" after the connection is successful.

3. Track Last Transaction Address:
   - The third button labeled "TRACK LAST ADDRESS" retrieves the recipient address of the latest transaction on the connected blockchain.
   - The JavaScript function "trackAddress()" is called when the button is clicked.
   - The function fetches the latest block number, gets the first transaction from the block (if any), and displays the recipient address in the paragraph with the id "addressArea."

4. Get Connected Sites and Network:
   - The fourth and fifth buttons labeled "GET CONNECT SITES" and "GET CONNECT NETWORK" interact with the smart contract to retrieve data.
   - Clicking these buttons triggers the JavaScript functions "getNumberOfSites()" and "getConnectedNetwork()" respectively.
   - The functions fetch the corresponding information from the smart contract and display it in the paragraphs with the ids "getsiteArea" and "networkArea" respectively.

Overall, this HTML file showcases a simple user interface that allows users to interact with the MetaMask wallet and the deployed smart contract, providing an overview of how to integrate a web application with Ethereum blockchain data using Web3.js and MetaMask.

## Lincense 

This project is licensed under the MIT License.












 
