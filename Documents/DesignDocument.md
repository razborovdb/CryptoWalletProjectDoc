# Crypto Wallet Project Design

## Description

This project provides:

* Creating, retrieving, and updating cryptocurrency wallets
* Retrieving all customer wallets in the list

This project uses authorization to validate users.

This project uses PostgresSQL as Database, SpringBoot as server, Android application as frontend.

## 1. Problem Statement

Today, many people have cryptocurrencies as an asset. At the same time,
the number of types of cryptocurrencies that they can own may be in the tens.
They can mine, buy and exchange cryptocurrencies and also store cryptocurrencies
on different cryptocurrency exchanges. Users want to have information about the assets
that they store in cryptocurrencies in one place.
Users also want to distribute their assets to different wallets in order
to accumulate the necessary assets for different projects.

This design document describes the Crypto Wallet Project, a new project
that will provide the custom wallet functionality to meet customers' needs.
It is designed to interact with the Crypto Wallet Project client,
which allows customers to get information about their wallets to their
phone or computer.

## 2. Use Cases

U1. As a customer, I want to create a new, empty wallet with a given name and
a description.

U2. As a customer, I want to retrieve all my wallets list.

U3. As a customer, I want to retrieve my wallet with a given name.

U4. As a customer, I want to update my wallet description.

U5. As a customer, I want to add cryptocurrency to my wallet.

U6. As a customer, I want to update a cryptocurrency amount in my wallet.

U7. As a customer, I want to update the cryptocurrency exchange rate in all my wallets.

U8. As a customer, I want to retrieve all available cryptocurrencies that I can use
in my wallet.

## 3. Project Scope

### 3.1. In Scope

* Creating, retrieving, and updating a wallet
* Retrieving all customer wallets in the list
* Retrieving all available cryptocurrencies that we can use in our wallet
* Updating cryptocurrencies exchange rate from external API (at this stage,
  the available cryptocurrencies list and the cryptocurrencies exchange rate will be updated
  by the system administrator).

# 4. Proposed Architecture Overview

This initial iteration will provide the minimum lovable product (MLP) including:
* Creating, retrieving, and updating a wallet
* Retrieving all customer wallets in the list
* Retrieving all available cryptocurrencies that we can use in our wallet

We will use https://render.com/ to deploy SpringBoot application where we  
create endpoints that will handle the creation, update, and retrieval of wallets to satisfy our requirements.

We will use https://render.com/ to deploy PostgresSQL database. In this database we will store 
available cryptocurrencies that we can use in our wallet in a table in PostgresSQL. Wallets
themselves will also be stored in PostgresSQL. For simpler cryptocurrency list retrieval, we
will store the list of cryptocurrencies in a given wallet directly in the wallet
table.

Crypto Wallet Project will also provide a Android application for users to manage their wallets.
A home page providing a list view of all of customer wallets will let them create new wallets and link off
to pages, per-wallet to update metadata, add cryptocurrencies and change their amount in the wallet.

# 5. API

## 5.1. Public Models

```
// UsersModel

String email;
String name;
String avatar;
String avatarUrl;
String password;
String role;
Boolean isAdmin;
```

```
// CryptosModel

String cryptoName;
String cryptoType;
String image;
String imageUrl;
String cryptoDescription;
Double cryptoAmount;
Double cryptoCost;
```

```
// WalletModel

String userId;
String walletName;
String walletDescription;
Double cryptosCount;
Double cryptosCost;
List   CryptosModel;
```

```
// ProjectsModel

String projectName;
String image;
String imageUrl;
String projectDescription;
Double projectCost;
```

----------------------------------------------------
## 5.2. RegisterUser Endpoint

* Accepts `POST` requests to `/user/register`
* Accepts a User with all parameters and returns token.
    * If the given email in User is found, will return an error.
    * If user created, return token.

## 5.3. LoginUser Endpoint

* Accepts `POST` requests to `/user/login`
* Accepts a User with email and returns token.
    * If the User with given email is not found, will return an error.
    * If User is found, return token.

## 5.4. GetAvailableCrypto Endpoint

* Accepts `GET` requests to `/api/cryptos`
* Accepts token and returns the list of available cryptos.

## 5.5. DeleteCryptoInAvailableCrypto Endpoint

* Accepts `DELETE` requests to `/api/crypto`
* Accepts token and CryptoCurrencies with cryptoName returns cryptoName.

## 5.6. AddCryptoToAvailableCrypto Endpoint

* Accepts `POST` requests to `/api/crypto`
* Accepts token and CryptoCurrencies with all parameters returns added crypto.

## 5.7. UpdateCryptoInAvailableCryptocurrencies Endpoint

* Accepts `PUT` requests to `/api/crypto`
* Accepts token and CryptoCurrencies with cryptoName returns updated crypto.

## 5.8. GetCryptoFromAvailableCryptocurrencies Endpoint

* Accepts `GET` requests to `/api/crypto`
* Accepts token and cryptoName as request param, returns crypto.

## 5.9. GetUserWallets Endpoint (made)

* Accepts `GET` requests to `/api/wallets`
* Accepts token and email as request param, returns user wallets list.

## 5.10. AddWallet Endpoint (made)

* Accepts `POST` requests to `/api/wallet`
* Accepts token and Wallets with all parameters returns added wallet.


## 5.11. GetUserOneWallet Endpoint (made)

* Accepts `GET` requests to `/api/wallet`
* Accepts token, email and walletName as request param, returns user wallet.

## 5.12. DeleteUserWallet Endpoint (made)

* Accepts `DELETE` requests to `/api/wallet`
* Accepts token and Wallet with userId and walletName returns walletName.

## 5.13. UpdateUserWallet Endpoint (made)

* Accepts `PUT` requests to `/api/wallet`
* Accepts token and Wallet with all parameters returns Wallet.

## 5.14. AddCryptoToUserWallet Endpoint (made)

* Accepts `POST` requests to `/api/wallet/crypto`
* Accepts token, userId, walletName, CryptoCurrencies with all parameters in the request body returns user's Wallet.

## 5.15. UpdateCryptoInUserWallet Endpoint (made)

* Accepts `PUT` requests to `/api/wallet/crypto`
* Accepts token, userId, walletName, CryptoCurrencies with all parameters in the request body returns user's Wallet.

## 5.16. DeleteCryptoInUserWallet Endpoint

* Accepts `DELETE` requests to `/api/wallet/crypto`
* Accepts token, userId, walletName, CryptoCurrencies with all parameters in the request body returns cryptoName.


## 5.17. GetUser Endpoint (made)

* Accepts `GET` requests to `/api/user`
* Accepts token, email as path variable returns Users.

## 5.18. UpdateUser Endpoint

* Accepts `PUT` requests to `/api/user`
* Accepts token, Users as body returns Users.

## 5.19. GetProject Endpoint (made)

* Accepts `GET` requests to `/api/project`
* Accepts no parameters returns Projects.


# 6. Tables

### 6.1. `users`

```
email // partition key, ordinal 0, text
name // text
avatar // text
avatarurl // text
password // text
role // text
isadmin // boolean
```

### 6.2. `cryptosmodel`

```
String cryptoName;
String cryptoType;
String image;
String imageUrl;
String cryptoDescription;
Double cryptoAmount;
Double cryptoCost;
```

### 6.3. `cryptos`

```
cryptoName // partition key, ordinal 0, text
image // text
imageurl // text
cryptodescription // text
cryptoamount // double
cryptocost // double
```

### 6.4. `wallets`

```
userid // partition key, ordinal 0, text
walletname // partition key, ordinal 1, text
walletdescription // text
cryptoscount // double
cryptoscost // double
cryptocurrencieslist // list, udt (cryptocurrenciesmodel)
```

### 6.5. `projects`

```
projectname // partition key, ordinal 0, text
image // text
imageurl // text
projectdescription // text
projectcost // double
```

