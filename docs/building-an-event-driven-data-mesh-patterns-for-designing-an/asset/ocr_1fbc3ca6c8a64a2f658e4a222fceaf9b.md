# # Hybrid Events—State with a Bit of Delta

Hybrid events are a mixture of state and delta. It’s best to think of these as state events that may contain a bit of information about _why_ or _how_ something happened. Let’s look at an example for clarity.

Consider the following scenario. A company provides an online service that requires a user to sign up before using it. There are several that a user can sign up:

+   - Via the main sign up button on the home page
+   - Via an email advertising link
+   - Using a third-party account (a Google account, for example)
+   - The account was manually created for them by an administrator

The hybrid data product consumer wants to know _how_ the user signed up. For operational use cases, we want to know which onboarding workflow to serve them when they next log in. For analytical purposes, we want to know which of our methods of sign-up are the most common so we can allocate our development resources.

One way to model this sign-up is with a user state event, with a single enumeration indicating the sign-up mechanism (after all, you can only sign up once!). An example of the record would look like this:

```
Key: "USERID-9283716596927463"
Value: {
  name: "Randolf T. Bandit"
  signup_time: "2022-02-22T22:22:22Z"
  birthday: "2000-01-01T00:00:00Z"
  // An enum of (MAIN, VIA_AD_EMAIL, THIRD_PARTY, or ADMIN)
  method_of_signup: "VIA_AD_EMAIL"
}
```

To create the hybrid event, we incorporated what would otherwise be delta events into a single state event. Instead of `signed_up_via_email`, `signed_up_via_homepage`, `signed_up_via_third_party`, and `signed_up_via_admin` events, we flattened them down into a single enum and appended them to the user entity. The domain of values in the user state event needs to account for each of the possible enum settings: for example, we may also want to include information about which third-party sign-in provider was used or which email campaign got the user to sign up.

And herein lies the main issue with hybrid events. The precise mechanism of _how_ something came to be in a domain is by and large a private detail, but by exposing this information we also expose the internal business logic process for coupling on by downstream consumers.