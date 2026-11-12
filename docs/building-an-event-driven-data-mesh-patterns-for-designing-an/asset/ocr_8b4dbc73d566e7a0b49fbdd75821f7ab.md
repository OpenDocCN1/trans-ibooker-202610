# Table 4-1. Using field-level encryption to partially encrypt an event

| Field name | Original event | Partially encrypted event |
|---|---|---|
| email | adam@bellemare.com | n2Zl@p987NhB4.L0P |
| user | abellemare | 9ajkpZp2kH |
| account | VD8675309 | OP1wW81Mx |
| amount | $777.77 | $777.77 |
| datetime | 2022-02-22:22:22:22 | 2022-02-22:22:22:22 |

You may also choose to use format-preserving encryption to maintain the format of the event data. In this case, we used format-preserving encryption for the email, user, and account fields—the same alphanumeric characters, spacing, and character count of the original fields, but without exposing any of the PII to users without decryption permissions.

One of the advantages of using field-level encryption is that it permits finer-grained access controls for consumers. Your consumers can request decryption keys only for the data they need, instead of for the entire payload, reducing the potential for inadvertently leaked information.

Format-preserving encryption is particularly useful for applying encryption to data after the fact because you don't need to renegotiate the schemas with downstream consumers. In contrast, using nonformat preserving encryption often results in malformation, such as converting a long bank account ID into a 64-character string or encrypting a complex nested object into an array of hashed bytes.

Encryption of sensitive data, whether end-to-end or field-level, can also help us with another significant governance requirement: the right to be forgotten and have our data deleted.

# Data Privacy, the Right to Be Forgotten, and Crypto-Shredding

General Data Protection Regulation (GDPR) is (among other things) a law requiring the careful handling, storage, and deletion of data. It is an excellent example of a legal constraint that your organization may need to adhere to in order to stay on the right side of the law. And if you're looking to create a data mesh of useful data products, it's very likely you're going to end up dealing with personal, account, and financial information that may require you to take extra steps and precautions to secure.