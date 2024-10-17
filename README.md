# Vault Associate 002
---
> #### Q1: You are using the Vault userpass auth method mounted at auth/userpass.How do you create a new user named "sally" with password "h0wN0wB4r0wnC0w"? This new user will need the power-users policy.
- [ ] `vault put auth/userpass/users/sally password=h0wN0wB4r0wnC0w policies=power-users`
- [ ] `vault write userpass/sally password=h0wN0wB4r0wnC0w policies=power-users`
- [ ] `vault kv write userpass/sally password=h0wN0wB4r0wnC0w policies=power-users`
- [ ] `vault write auth/userpass/users/sally password=h0wN0wB4r0wnC0w policies=power-users`
<details>
  <summary> Answer </summary>
  
  `vault put auth/userpass/users/sally password=h0wN0wB4r0wnC0w policies=power-users`
  
</details>

> #### Q2: The Vault lease renew command increments the lease time from:
- [ ] The current time
- [ ] The end of the lease
<details>
  <summary> Answer </summary>
  
  The Current Time, not the end of the lease. This means that the user can request a specific amount of time they want remaining on the lease, termed the increment. This is not an increment at the end of the current TTL; it is an increment from the current time. For example, `vault lease renew -increment=3600 my-lease-id` would request that the TTL of the lease be adjusted to 1 hour(3600 seconds) from now. Having the increment be rooted at the current time instead of the end of the lease makes it easy for users to reduce the length of leases if they don't actually need credentials for the full possible lease period, allowing those credentials to expire sooner and resources to be cleaned up earlier. The requested increment is completely advisory. The backend in charge of the secret can choose to completely ignore it.
  Refrence: Lease, Renew, and Revoke | Vault | HashiCorp Developer
  
</details>

> #### Q3: HOTSPOT Where do you define the Namespace to log into using the Vault UI?
![alt text](signintovault.png)

<details>
  <summary> Answer </summary>
  
  The namespace can be defined in the "Mount Path" field in the "Advance Options" section of the login screen. The mount Path is the path where the auth method is enabled, and it can include a namespace prefix. For example LDAP auth method is enabled at the path `ns1/auth/ldap`, where `ns1` is the namespace, then the mount path field should be set to `ns1/auth/ldap` this way, the Vault UI will log in to the correct namespace and auth method. Alternatively, the namespace can also be specified in the URL of the Vault UI, such as 
 ***https://vault.example.com/ui/vault/auth/ns1/auth/ldap/login***

</details>

> #### Q4: You have a 2GB Base64 binary large object (blob) that needs to be encrypted. Which of the following best describes the transit secrets engine?
- [ ] A Data key encrypts the blob locally, and the same key decrypts the blob locally.
- [ ] To process such a large blob. Vault will temporarily store it in the storage backend.
- [ ] Vault will store the blob permanently. Be sure to run Vault on a compute optimized machine
- [ ] The transit engine is not a good solution for binaries of this size.
<details>
  <summary> Answer </summary>
  
  The transit engine is not a good solution for binaries of this size, because it is designed to handle cryptographic functions on data in-transit, not data at-rest. The transit secrets engine does not store any data sent to it, so it would require sending the entire 2GB blob to Vault for encryption or decryption,
  which would be inefficient and impractical. A better solution would be to use the transit secrets engine to generate a data key, which is a high-entropy key that can be used to encrypt or decrypt data locally. The data key can be returned in plaintext or wrapped by another key, depending on the use case. This way, the transit secrets engine only handles the encryption or decryption of the data key, not the data itself, and the data can be stored in any primary data store.

  Reference: Transit - Secrets Engines | Vault | HashiCorp Developer, Encryption as a service: transit secrets engine | Vault | HashiCorp Developer
</details>

> #### Q5: How would you describe the value of using the Vault Transit secrets engine?
- [ ] Vault has an API that can be programmatically consumed by applications
- [ ] The transit secrets engine ensures encryption in-transit and at-rest is enforced enterprise wide
- [ ] Encryption for application data is best handled by a storage system or database engine, while storing encryption keys in Vault
- [ ] The transit secrets engine relieves the burden of proper encryption/decryption from application developers and pushes the burden onto the operators of Vault
<details>
  <summary> Answer </summary>
  
  The transit secrets engine relieves the burden of proper encryption/decryption from application developers and pushes the burden onto the operators of Vault. The transit secrets engine provides encryption as a service, which means that it performs cryptographic operations on data in-transit without storing any data. This allows developers to delegate the responsibility of managing encryption keys and algorithms to Vault operators, who can define and enforce policies on the transit secrets engine. This way, developers can focus on their application logic and data, while Vault handles the encryption and decryption of data in a secure and scalable manner.

  Reference: Transit - Secrets Engines | Vault | HashiCorp Developer, Encryption as a service: transit
  secrets engine | Vault | HashiCorp Developer
</details>

> #### Q6: What is the Vault CLI command to query information about the token the client is currently using?

- [ ] `vault lookup token`
- [ ] `vault token lookup`
- [ ] `vault lookup self`
- [ ] `vault self-lookup`
<details>
  <summary> Answer </summary>
  
  `vault token lookup` The Vault CLI command to query information about the token the client is currently using is vault token lookup. This command displays information about the token or accessor provided as an argument, or the locally authenticated token if no argument is given. The information includes the token ID, accessor, policies, TTL, creation time, and metadata. This command can be useful for debugging and auditing purposes, as well as for renewing or revoking tokens.

Reference: token lookup - Command | Vault | HashiCorp Developer, Tokens | Vault | HashiCorp Developer
</details>

> #### Q7: Which of the following is a machine-oriented Vault authentication backend?

- [ ] Okta
- [ ] AppRole
- [ ] Transit
- [ ] GitHub
<details>
  <summary> Answer </summary>
  
  AppRole is a machine-oriented authentication method that allows machines or applications to authenticate with Vault using a role ID and a secret ID. The role ID is a unique identifier for the application, and the secret ID is a single-use credential that can be delivered to the application securely. AppRole is designed to provide secure introduction of machines and applications to Vault, and to support the principle of least privilege by allowing fine-grained access control policies to be attached to each role. Okta, GitHub, and Transit are not machine-oriented authentication methods. Okta and GitHub are useroriented authentication methods that allow users to authenticate with Vault using their Okta or GitHub credentials. Transit is not an authentication method at all, but a secrets engine that provides encryption as a service.

Reference: AppRole Auth Method | Vault | HashiCorp Developer Okta Auth Method | Vault | HashiCorp Developer GitHub Auth Method | Vault | HashiCorp Developer Transit Secrets Engine | Vault | HashiCorp Developer
</details>

> #### Q8: Security requirments demand that no secrets appear in the shell history. Which command does not meet this requirment?

- [ ] `generate-password | vault kv put secret/password value`
- [ ] `vault kv put secret/password value-itsasecret`
- [ ] `vault kv put secret/password value=@data.txt`
- [ ] `vault kv put secret/password value-SSECRET_VALUE`
<details>
  <summary> Answer </summary>
 
  `vault kv put secret/password value-itsasecret`
   This command would store the secret value `itsasecret` in the `key/value` secrets engine at the path `secret/password`, but it would also expose the secret value in the shell history, which could be accessed by other users or malicious actors. This is not a secure way of storing secrets in Vault. The other commands are more secure ways of storing secrets in Vault without revealing them in the shell history.



</details>

> #### Q9: You can build a high availability Vault cluster with any storage backend.

- [ ] True
- [ ] False
<details>
  <summary> Answer </summary>
 
  False
   Not all storage backends support high availability mode for Vault. Only the storage backends that support locking can enable Vault to run in a multi-server mode where one server is active and the others are standby. Some examples of storage backends that support high availability mode are Consul, Integrated Storage, and ZooKeeper. Some examples of storage backends that do not support high availability mode are Filesystem, MySQL, and PostgreSQL.
  
  Reference:
  https://developer.hashicorp.com/vault/docs/concepts/ha1,
  https://developer.hashicorp.com/vault/docs/configuration/storage2
   
</details>

> #### Q10: What command creates a secret with the `my-password` and the value `53cr3t` at path `my-secrets` within `KV` secrets engine mounted at `secret`

- [ ] `vault kv put secret/my-secrets/my-password 53cr3t`
- [ ] `vault kv write secret/my-secrets/my-password 53cr3t`
- [ ] `vault kv write 53cr3t my-secrets/my-password`
- [ ] `vault kv put secret/my-secrets »y-password-53cr3t`
<details>
  <summary> Answer </summary>
  
  `vault kv put secret/my-secrets/my-password 53cr3t`
   or `vault kv put secret/my-secrets my-password=53cr3t`

   Reference: https://developer.hashicorp.com/vault/docs/commands/kv/put3,
              https://developer.hashicorp.com/vault/docs/commands/kv4
</details>

> #### Q11: What can be used to limit the scope of a credential breach?
- [ ] Storage of secrets in a distributed ledger
- [ ] Enable audit logging 
- [ ] Use of a short-lived dynamic secrets
- [ ] Sharing credentials between applications
<details>
  <summary> Answer </summary>
  
  Use of a short-lived dynamic secrets
</details>