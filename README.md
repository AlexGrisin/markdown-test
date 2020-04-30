# Report for apitests.AccountTestSuite

## Summary

* Total Runs: 0
* Success Rate: 100.0%
* Failures: 0
* Errors:   0
* Skipped:  14
* Total time: 0.001 seconds



## Features

### Customer should be able to login with facebook account

Result: **IGNORED**
Time: 0

<details>
<summary> When: attempt to log in with facebook account </summary>

<p>

```java
String accessToken = login(FB_ACCOUNT)
def loginResponse = accountApi.loginByEmailFB(uid, accessToken)
```

</p>

</details>

<details>
<summary> Then: login is successful </summary>

<p>

```java
loginResponse.httpStatus == SC_OK
```

</p>

</details>

### Customer should be able to log in with registered account

Result: **IGNORED**
Time: 0

<details>
<summary> When: registered account attempts to log in </summary>

<p>

```java
String accessToken = login(uid)
```

</p>

</details>

<details>
<summary> Then: login is successful </summary>

<p>

```java
accessToken != null
```

</p>

</details>

### Customer should be able to log out with registered account

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account logged in </summary>

<p>

```java
String accessToken = login(uid)
```

</p>

</details>

<details>
<summary> When: attempts to log out </summary>

<p>

```java
def logoutResponse = accountApi.logout(uid, accessToken)
```

</p>

</details>

<details>
<summary> Then: logout is successful </summary>

<p>

```java
logoutResponse.httpStatus == SC_OK
```

</p>

</details>

### Customer should be able to obtain my account details

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account logged in </summary>

<p>

```java
String accessToken = login(uid)
```

</p>

</details>

<details>
<summary> When: requests my account details </summary>

<p>

```java
def myProfileResponse = accountApi.myProfile(uid, accessToken)
```

</p>

</details>

<details>
<summary> Then: my account details are returned </summary>

<p>

```java
myProfileResponse.httpStatus == SC_OK
jsonSlurper.parseText(myProfileResponse.body).name == uid
```

</p>

</details>

### Customer should not be able to log in with invalid credentials

Result: **IGNORED**
Time: 0

<details>
<summary> When: customer attempts to log in with invalid user/password combinations </summary>

<p>

```java
def loginResponse = accountApi.login(account, password)
```

</p>

</details>

<details>
<summary> Then: log in is refused </summary>

<p>

```java
loginResponse.httpStatus == SC_BAD_REQUEST
```

</p>

</details>

<details>
<summary> Where: different invalid user/password combinations </summary>

### Customer should be able to receive recently viewed range

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account logged in </summary>

<p>

```java
String accessToken = login(uid)
```

</p>

</details>

<details>
<summary> When: performs rdp range search </summary>

<p>

```java
rdpApi.searchAuthorized(RANGE_REVIEW, accessToken)
def myProfileResponse = accountApi.myProfile(uid, accessToken)
def recentlyViewedResponse = accountApi.recentlyViewed(accessToken)
```

</p>

</details>

<details>
<summary> Then: searched range is displayed in my account in recently viewed section </summary>

<p>

```java
responseContains(myProfileResponse.body, RANGE_REVIEW)
responseContains(recentlyViewedResponse.body, RANGE_REVIEW)
```

</p>

</details>

### Customer should be able to add address to account

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account logged in </summary>

<p>

```java
String accessToken = login(uid)
```

</p>

</details>

<details>
<summary> When: adds customer address </summary>

<p>

```java
def response = accountApi.addAddress(uid, accessToken, addressPayload)
```

</p>

</details>

<details>
<summary> Then: address is added to account </summary>

<p>

```java
response.httpStatus == SC_CREATED
responseContains(response.body, '"lastName" : "TKLastName"')
```

</p>

</details>

### Customer should be able to retrieve stored account address

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account with address added logged in </summary>

<p>

```java
String accessToken = login(uid)
accountApi.addAddress(uid, accessToken, addressPayload)
```

</p>

</details>

<details>
<summary> When: requests customer address </summary>

<p>

```java
def getResponse = accountApi.getAddress(uid, accessToken)
```

</p>

</details>

<details>
<summary> Then: customer address is returned </summary>

<p>

```java
getResponse.httpStatus == SC_OK
responseContains(getResponse.body, '"lastName" : "TKLastName"')
```

</p>

</details>

### Customer should be able to delete address from account

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account with address added logged in </summary>

<p>

```java
String accessToken = login(uid)
accountApi.addAddress(uid, accessToken, addressPayload)
```

</p>

</details>

<details>
<summary> When: deletes address from address book </summary>

<p>

```java
def getResponse = accountApi.getAddress(uid, accessToken)
String addressId = jsonSlurper.parseText(getResponse.body).addresses[0].id
def deleteResponse = accountApi.deleteAddress(uid, addressId, accessToken)
getResponse = accountApi.getAddress(uid, accessToken)
```

</p>

</details>

<details>
<summary> Then: address is removed and no longer available for account </summary>

<p>

```java
deleteResponse.httpStatus == SC_OK
!responseContains(getResponse.body, '"lastName" : "TKLastName"')
```

</p>

</details>

### Customer should be able to submit customer review

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account logged in </summary>

<p>

```java
String accessToken = login(uid)
String timestamp = timestamp()
String payload = preparePayload(USER_REVIEW_PAYLOAD, [
        timestamp: timestamp,
        uid      : uid
])
```

</p>

</details>

<details>
<summary> When: submits customer review </summary>

<p>

```java
def reviewResponse = accountApi.submitReview(accessToken, payload)
def reviewPk = adminApi.flexSearch('findCustomerReviewByHeadline.fxs', [headline: timestamp])
```

</p>

</details>

<details>
<summary> Then: customer review is stored in the database </summary>

<p>

```java
reviewResponse.httpStatus == SC_CREATED
reviewPk != null
```

</p>

</details>

### Customer should be able to get submitted customer review

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered account with customer review submitted logged in </summary>

<p>

```java
String accessToken = login(uid)
String timestamp = timestamp()
String payload = preparePayload(USER_REVIEW_PAYLOAD, [
        timestamp: timestamp,
        uid      : uid
])
accountApi.submitReview(accessToken, payload)
def reviewPk = adminApi.flexSearch('findCustomerReviewByHeadline.fxs', [headline: timestamp])
adminApi.impexImport('approveCustomerReview.impex', [pk: reviewPk])
```

</p>

</details>

<details>
<summary> When: requests customer review </summary>

<p>

```java
def reviewResponse = accountApi.getReview(accessToken)
def review = jsonSlurper.parseText(reviewResponse.body).reviews.find { it.headline.contains(timestamp) }
```

</p>

</details>

<details>
<summary> Then: customer review received with all details </summary>

<p>

```java
reviewResponse.httpStatus == SC_OK
review.category == 'Leather'
review.comment == 'Here is the customer\'s comments for the review. Can be a long text'
review.rating == 1.0
```

</p>

</details>

### Employee with customer manager role should be able to update customer email

Result: **IGNORED**
Time: 0

<details>
<summary> Given: login with employee with customer manage role </summary>

<p>

```java
String accessToken = login(CUSTOMER_MANAGER)
String newUid = "new_${uid}"
```

</p>

</details>

<details>
<summary> When: updates customer email address/uid </summary>

<p>

```java
def response = accountApi.updateEmail(uid, newUid, accessToken)
def oldCustomerPk = adminApi.flexSearch('getCustomerByUid.fxs', [uid: uid])
def newCustomerPk = adminApi.flexSearch('getCustomerByUid.fxs', [uid: newUid])
```

</p>

</details>

<details>
<summary> Then: customer email/uid is updated </summary>

<p>

```java
response.httpStatus == SC_OK
oldCustomerPk.isEmpty()
!newCustomerPk.isEmpty()
```

</p>

</details>

### Existing customer should have current store associated with account

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered existing account logged in </summary>

<p>

```java
String accessToken = login(SOFOLOGIST_USER)
```

</p>

</details>

<details>
<summary> When: request to get current store </summary>

<p>

```java
def response = storeApi.getCurrent(accessToken)
```

</p>

</details>

<details>
<summary> Then: customer selected store is received </summary>

<p>

```java
response.httpStatus == SC_OK
jsonSlurper.parseText(response.body).name == SOFOLOGIST_STORE_NAME
```

</p>

</details>

### New customer should have current store set as default

Result: **IGNORED**
Time: 0

<details>
<summary> Given: registered new account logged in </summary>

<p>

```java
String accessToken = login(uid)
```

</p>

</details>

<details>
<summary> When: request to get current store </summary>

<p>

```java
def response = storeApi.getCurrent(accessToken)
```

</p>

</details>

<details>
<summary> Then: default store is received </summary>

<p>

```java
response.httpStatus == SC_OK
jsonSlurper.parseText(response.body).name == DEFAULT_STORE_NAME
```

</p>

</details>


<small>Generated by <a href="https://github.com/renatoathaydes/spock-reports">Athaydes Spock Reports</a></small>
