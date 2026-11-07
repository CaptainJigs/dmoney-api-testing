# dMoney API - Test Cases

Road to SDET Batch 20 - Assignment on API Testing. All test cases are automated in `dMoney-Assignment.postman_collection.json`.

**Total: 30 test cases — all passed (93/93 assertions, Postman Runner, 07 Nov 2026)** (21 positive, 9 negative). Each negative test runs before its positive test so a failure cannot break the chain.

| ID | Module | Test Case | Type | Request | Assertions | Status |
|---|---|---|---|---|---|---|
| TC01 | Admin Login | Admin login with wrong password | Negative | `POST /user/login` | Status code is 401<br>Message is 'Password incorrect'<br>No token is returned | Pass |
| TC02 | Admin Login | Admin login successfully | Positive | `POST /user/login` | Status code is 200<br>Message is 'Login successful'<br>Role is Admin and token exists | Pass |
| TC03 | Create Users | Create user without secret key | Negative | `POST /user/create` | Status code is 401<br>Message is 'Secret auth key validation failure!' | Pass |
| TC04 | Create Users | Create user with invalid role | Negative | `POST /user/create` | Status code is 400<br>Message says the role is invalid | Pass |
| TC05 | Create Users | Create Customer 1 | Positive | `POST /user/create` | Status code is 201<br>Message is 'User created'<br>Role is Customer<br>Phone and email match the request<br>Password is not returned | Pass |
| TC06 | Create Users | Create Customer 2 | Positive | `POST /user/create` | Status code is 201<br>Message is 'User created'<br>Role is Customer<br>Phone and email match the request<br>Password is not returned | Pass |
| TC07 | Create Users | Create Agent | Positive | `POST /user/create` | Status code is 201<br>Message is 'User created'<br>Role is Agent<br>Phone and email match the request<br>Password is not returned | Pass |
| TC08 | Create Users | Create Merchant | Positive | `POST /user/create` | Status code is 201<br>Message is 'User created'<br>Role is Merchant<br>Phone and email match the request<br>Password is not returned | Pass |
| TC09 | Activate Users (Admin) | Activate Customer 1 | Positive | `PATCH /user/update/{{customer1Id}}` | Status code is 200<br>Message is 'User updated successfully'<br>User status is active | Pass |
| TC10 | Activate Users (Admin) | Activate Customer 2 | Positive | `PATCH /user/update/{{customer2Id}}` | Status code is 200<br>Message is 'User updated successfully'<br>User status is active | Pass |
| TC11 | Activate Users (Admin) | Activate Agent | Positive | `PATCH /user/update/{{agentId}}` | Status code is 200<br>Message is 'User updated successfully'<br>User status is active | Pass |
| TC12 | Activate Users (Admin) | Activate Merchant | Positive | `PATCH /user/update/{{merchantId}}` | Status code is 200<br>Message is 'User updated successfully'<br>User status is active | Pass |
| TC13 | Login (System, Agent, Customers) | System account login | Positive | `POST /user/login` | Status code is 200<br>Login successful and token exists | Pass |
| TC14 | Login (System, Agent, Customers) | Agent login (OTP sent) | Positive | `POST /user/login` | Status code is 200<br>OTP is required | Pass |
| TC15 | Login (System, Agent, Customers) | Agent verify with wrong OTP | Negative | `POST /user/verify-otp` | Status code is 401<br>Message is 'Invalid OTP. Please try again.' | Pass |
| TC16 | Login (System, Agent, Customers) | Agent verify OTP | Positive | `POST /user/verify-otp?env=dev` | Status code is 200<br>Login successful with role Agent | Pass |
| TC17 | Login (System, Agent, Customers) | Customer 1 login (OTP sent) | Positive | `POST /user/login` | Status code is 200<br>OTP is required | Pass |
| TC18 | Login (System, Agent, Customers) | Customer 1 verify OTP | Positive | `POST /user/verify-otp?env=dev` | Status code is 200<br>Login successful with role Customer | Pass |
| TC19 | Login (System, Agent, Customers) | Customer 2 login (OTP sent) | Positive | `POST /user/login` | Status code is 200<br>OTP is required | Pass |
| TC20 | Login (System, Agent, Customers) | Customer 2 verify OTP | Positive | `POST /user/verify-otp?env=dev` | Status code is 200<br>Login successful with role Customer | Pass |
| TC21 | Transactions | System deposit to a Customer | Negative | `POST /transaction/deposit` | Status code is 400<br>Error message is correct<br>No transaction id is returned | Pass |
| TC22 | Transactions | System deposits 5000 tk to Agent | Positive | `POST /transaction/deposit` | Status code is 201<br>Message is 'SYSTEM deposit to Agent successful'<br>Amount is 5000 and agent balance is 5000<br>Transaction id exists | Pass |
| TC23 | Transactions | Agent deposit below minimum amount | Negative | `POST /transaction/deposit` | Status code is 400<br>Error message is correct<br>No transaction id is returned | Pass |
| TC24 | Transactions | Agent deposits 2000 tk to Customer 1 (assert commission) | Positive | `POST /transaction/deposit` | Status code is 201<br>Message is 'Deposit successful'<br>Deposit commission is 2.5% (50 tk)<br>Agent balance = 5000 - 2000 + 50 = 3050 | Pass |
| TC25 | Transactions | Customer sends money to own account | Negative | `POST /transaction/sendmoney` | Status code is 400<br>Error message is correct<br>No transaction id is returned | Pass |
| TC26 | Transactions | Customer 1 sends 1000 tk to Customer 2 (assert fee) | Positive | `POST /transaction/sendmoney` | Status code is 201<br>Message is 'Send money successful'<br>Service fee is 5 tk<br>Customer 1 balance = 2000 - 1000 - 5 = 995 | Pass |
| TC27 | Transactions | Customer cash out below minimum amount | Negative | `POST /transaction/withdraw` | Status code is 400<br>Error message is correct<br>No transaction id is returned | Pass |
| TC28 | Transactions | Customer 2 cashes out 500 tk from Agent (assert fee) | Positive | `POST /transaction/withdraw` | Status code is 201<br>Message is 'Withdraw successful'<br>Service fee is 1% with 5 tk minimum (5 tk)<br>Customer 2 balance = 1000 - 500 - 5 = 495 | Pass |
| TC29 | Transactions | Customer pays to a non-merchant account | Negative | `POST /transaction/payment` | Status code is 400<br>Error message is correct<br>No transaction id is returned | Pass |
| TC30 | Transactions | Customer 2 pays 400 tk to Merchant (assert fee) | Positive | `POST /transaction/payment` | Status code is 201<br>Message is 'Payment successful'<br>Service fee is 5 tk (minimum fee)<br>Customer 2 balance = 495 - 400 - 5 = 90 | Pass |
