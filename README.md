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
  
  Use of a short-lived dynamic secrets Dynamic secrets are generated on-demand by Vault and automatically revoked when they are no longer needed. This way, the credentials are not stored in plain text or in a static database, and they can be rotated frequently to prevent unauthorized access. Dynamic secrets also provide encryption as a service, which means that they perform cryptographic operations on data in transit without storing any data. This adds an extra layer of security and reduces the risk of data leakage or tampering.

  Reference: Dynamic secrets | Vault | HashiCorp Developer, What are dynamic secrets and why do I need them? - HashiCorp
</details>

> #### Q12: What environment variable overrides the CLI's default Vault server address?

- [ ] `VAULT_ADDR`
- [ ] `VAULT_HTTP_ADORESS`
- [ ] `VAULT_ADDRESS`
- [ ] `VAULT_HTTPS_ADDRESS`
<details>
  <summary> Answer </summary>
  
  `VAULT_ADDR`
  This environment variable can be set to the URL of the Vault server, which is used by Vault's CLI tool to communicate with the Vault server.
</details>

> #### Q13: Which of the following statements describe the CLI command below?
`vault login -method-ldap username-mitche11h`
- [ ] Generates a token which is respone wrapped
- [ ] You will be prompted to enter the password
- [ ] By default the gernerated token is valid for 24 hours
- [ ] Fails because the password is not provided
<details>
  <summary> Answer </summary>
  
  This command is a command to log in to Vault using the LDAP method. Usually, after executing this command, the user is prompted to enter their password, rather than the command immediately failing because the password was not provided. `vault login -method=ldap username=mitchellh`
</details>

> #### Q14: The following three policies exist in Vault. What do these policies allow an oraginization to do?
![alt text](policies.png)
- [ ] Separates permissions allowed on actions associated with the transit secret engine
- [ ] Nothing, as the minimum permissions to perform useful tasks are not present
- [ ] Encrypt, decrypt, and rewrap data using the transit engine all in one policy
- [ ] Create a transit encryption key for encrypting, decrypting, and rewrapping encrypted data
<details>
  <summary> Answer </summary>
  Separates permissions allowed on actions associated with the transit secret engine
  These policies allow organizations to:
  Separates permissions allowed on actions associated with the transit secret engine Here's how to do it: app.hcl The policy allows the entity to perform cryptographic operations using a specific key () of the Transit secret engine my_app_key callcenter.hcl The policy allows decryption operations to be performed on the same my_app_key rewrap.hcl Policies allow the key to be read and the data to be reencapsulated, which essentially decrypts and re-encrypts the data without displaying plaintext, which is useful for rotating the underlying encryption key. Each policy targets specific operations of the Transit secret engine, enabling fine-grained access control to encryption,decryption, and key management functions. This is important for maintaining a strict separation of duties within the organization.

</details>

> #### Q15: Your DevOps team would like to provision VMs in GCP via CICD pipeline. They would like to integrate Vault to protect the credentials used by the tool. Which secrets engine would you recommend?

- [ ] Google Cloud Secrets Engine
- [ ] Identity secrets engine
- [ ] Key/Value secrets engine version 2
- [ ] SSH secrets engine
<details>
  <summary> Answer </summary>
 
  The Google Cloud Secrets Engine is the best option for the DevOps team to provision VMs in GCP via a CICD pipeline and integrate Vault to protect the credentials used by the tool. The Google Cloud Secrets Engine can dynamically generate GCP service account keys or OAuth tokens based on IAM policies, which can be used to authenticate and authorize the CICD tool to access GCP resources. The credentials are automatically revoked when they are no longer used or when the lease expires, ensuring that the credentials are short-lived and secure. The DevOps team can configure rolesets or static accounts in Vault to define the scope and permissions of the credentials, and use the Vault API or CLI to request credentials on demand. The Google Cloud Secrets Engine also supports generating access tokens for impersonated service accounts, which can be useful for delegating access to other serviceaccounts without storing or managing their keys. The Identity Secrets Engine is not a good option for this use case, because it does not generate GCP credentials, but rather generates identity tokens that can be used to access other Vault secrets engines or namespaces. The Key/Value Secrets Engine version 2 is also not a good option, because it does not
  generate dynamic credentials, but rather stores and manages static secrets that the user provides. The SSH Secrets Engine is not a good option either, because it does not generate GCP credentials, but rather generates SSH keys or OTPs that can be used to access remote hosts via SSH.

  Reference: Google Cloud - Secrets Engines | Vault | HashiCorp Developer Identity - Secrets Engines | Vault | HashiCorp Developer KV - Secrets Engines | Vault
</details>

> #### Q15: Your DevOps team would like to provision VMs in GCP via CICD pipeline. They would like to integrate Vault to protect the credentials used by the tool. Which secrets engine would you recommend?

- [ ] Google Cloud Secrets Engine
- [ ] Identity secrets engine
- [ ] Key/Value secrets engine version 2
- [ ] SSH secrets engine
<details>
  <summary> Answer </summary>
 
  The Google Cloud Secrets Engine is the best option for the DevOps team to provision VMs in GCP via a CICD pipeline and integrate Vault to protect the credentials used by the tool. The Google Cloud Secrets Engine can dynamically generate GCP service account keys or OAuth tokens based on IAM policies, which can be used to authenticate and authorize the CICD tool to access GCP resources. The credentials are automatically revoked when they are no longer used or when the lease expires, ensuring that the credentials are short-lived and secure. The DevOps team can configure rolesets or static accounts in Vault to define the scope and permissions of the credentials, and use the Vault API or CLI to request credentials on demand. The Google Cloud Secrets Engine also supports generating access tokens for impersonated service accounts, which can be useful for delegating access to other serviceaccounts without storing or managing their keys. The Identity Secrets Engine is not a good option for this use case, because it does not generate GCP credentials, but rather generates identity tokens that can be used to access other Vault secrets engines or namespaces. The Key/Value Secrets Engine version 2 is also not a good option, because it does not generate dynamic credentials, but rather stores and manages static secrets that the user provides. The SSH Secrets Engine is not a good option either, because it does not generate GCP credentials, but rather generates SSH keys or OTPs that can be used to access remote hosts via SSH.

  Reference: Google Cloud - Secrets Engines | Vault | HashiCorp Developer Identity - Secrets Engines | Vault | HashiCorp Developer KV - Secrets Engines | Vault
</details>

> #### Q16: Which of these is not a benefit of dynamic secrets?

- [ ] Supports systems which do not natively provide a method of expiring credentials
- [ ] Minimizes damage of credentials leaking
- [ ] Ensures that administrators can see every password used
- [ ] Replaces cumbersome password rotation tools and practices
<details>
  <summary> Answer </summary>

Ensures that administrators can see every password use. Dynamic secrets are generated on-demand by Vault and have a limited time-to-live (TTL). THEY DO NOT
ensure that administrators can see every password used, as they are often encrypted and ephemeral. 
The benefits of dynamic secrets are:
- They support systems that do not natively provide a method of expiring credentials, such as databases,
cloud providers, SSH, etc. 
- Vault can revoke the credentials when they are no longer needed or when the lease expires.
- They minimize the damage of credentials leaking, as they are short-lived and can be easily rotated or revoked. If a credential is compromised, the attacker has a limited window of opportunity to use it before
it becomes invalid.
- They replace cumbersome password rotation tools and practices, as Vault can handle the generation and revocation of credentials automatically and securely. This reduces the operational overhead and complexity of managing secrets.

  Reference:
  https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-dynamic-secrets1,
  https://developer.hashicorp.com/vault/docs/concepts/lease2

</details>

> #### Q17: Which of the following cannot define the maximum time-to-live (TTL) for a token?

- [ ] By the authentication method t natively provide a method of expiring credentials
- [ ] By the client system of credentials leaking
- [ ] By the mount endpoint configuration very password used
- [ ] A parent token TTL e password rotation tools and practices
- [ ] System max TTL
<details>
  <summary> Answer </summary>

  By the client system of credentials leaking, The maximum time-to-live (TTL) for a token is defined by the lowest value among the following factors:
  - The authentication method that issued the token. Each auth method can have a default and a maximum TTL for the tokens it generates. These values can be configured by the auth method’s mount options or by the auth method’s specific endpoints.
  - The mount endpoint configuration that the token is accessing. Each secrets engine can have a default and a maximum TTL for the leases it grants. These values can be configured by the secrets engine’s mount options or by the secrets engine’s specific endpoints.
  - A parent token TTL. If a token is created by another token, it inherits the remaining TTL of its parent token, unless the parent token has an infinite TTL (such as the root token). A child token cannot outlive its parent token.
  - System max TTL. This is a global limit for all tokens and leases in Vault. It can be configured by the system backend’s max_lease_ttl option.
  - The client system that uses the token cannot define the maximum TTL for the token, as this is determined by Vault’s configuration and policies. The client system can only request a specific TTL for the token, but this request is subject to the limits imposed by the factors above.

  Reference:
  https://developer.hashicorp.com/vault/docs/concepts/tokens3,
  https://developer.hashicorp.com/vault/docs/concepts/lease2,
  https://developer.hashicorp.com/vault/docs/commands/auth/tune4,
  https://developer.hashicorp.com/vault/docs/commands/secrets/tune5,
  https://developer.hashicorp.com/vault/docs/commands/token/create6

</details>

> #### Q18: What are orphan tokens?

- [ ] Orphan tokens are tokens with a use limit so you can set the number of uses when you create them
- [ ] Orphan tokens are not children of their parent; therefore, orphan tokens do not expire when their parent does
- [ ] Orphan tokens are tokens with no policies attached
- [ ] Orphan tokens do not expire when their own max TTL is reached
<details>
  <summary> Answer </summary>

  Orphan tokens are not children of their parent; therefore, orphan tokens do not expire when their parent does.
  An orphan token is a token that cannot be cascaded to be revoked when its parent token is revoked. Typically, when a parent token is revoked, all child tokens created by it are also revoked. However, orphan tokens are an exception, as they do not have this parent-child association and therefore remain active when the parent token is revoked.

</details>

> #### Q19: To give a role the ability to display or output all of the end points under the /secrets/apps/* end point it would need to have which capability set?

- [ ] update
- [ ] read
- [ ] sudo
- [ ] list
- [ ] None of the above
<details>
  <summary> Answer </summary>

  List - In Vault, permission is required to view or output all endpoints under a path. The permission allows the role to view all key names under the path, but does not provide the content of the key. “list”

</details>

> #### Q20: You have been tasked with writing a policy that will allow read permissions for all secrets at path `secret/bar`. The users that are assigned this policy should also be able to list the secrets. What should this policy look like?

- [ ] `path "secret/bar/*" {capabilities = ["read","list"]}`
- [ ] `path "secret/bar/*" {capabilities = ["list"]} path "secret/bar/" {capabilities = ["read"]}`
- [ ] `path "secret/bar/*" {capabilities = ["read"]} path "secret/bar/" {capabilities = ["list"]}`
- [ ] `path "secret/bar/+" {capabilities = ["read","list"]}`
<details>
  <summary> Answer </summary>

  ```
  path "secret/bar/*" {
  capabilities = ["read","list"]
  }
  ```
This policy will allow the user to read all the secrets in the read path and list all the secrets in that path. The asterisk (*) here is a wildcard that indicates all possible subpaths under the path. Such a policy setup ensures that the user can not only list all the secrets, but also read each one. `secret/bar/*`

</details>

> #### Q21: When using Integrated Storage, which of the following should you do to recover from possible data loss?

- [ ] Failover to a standby node
- [ ] Use snapshot
- [ ] Use audit logs
- [ ] Use server logs
<details>
  <summary> Answer </summary>

  Use snapshot, Integrated Storage is a Raft-based storage backend that allows Vault to store its data internally without relying on an external storage system. It also enables Vault to run in high availability mode with automatic leader election and failover. However, Integrated Storage is not immune to data loss or corruption due to hardware failures, network partitions, or human errors. Therefore, it is recommended to use the snapshot feature to backup and restore the Vault data periodically or on demand. A snapshot is a point-in-time capture of the entire Vault data, including the encrypted secrets, the configuration, and the metadata. Snapshots can be taken and restored using the `vault operator raft snapshot` command or the `sys/storage/raft/snapshot` API endpoint. Snapshots are encrypted and can only be restored with a quorum of unseal keys or recovery keys. Snapshots are also portable and can be used to migrate data between different Vault clusters or storage backends.

  Reference:
  https://developer.hashicorp.com/vault/docs/concepts/integrated-storage1,
  https://developer.hashicorp.com/vault/docs/commands/operator/raft/snapshot2,
  https://developer.hashicorp.com/vault/api-docs/system/storage/raft/snapshot3
</details>

> #### Q22: How many Shamir's key shares are required to unseal a Vault instance?

- [ ] All key shares
- [ ] A quorum of key shares
- [ ] One or more keys
- [ ] The threshold number of key shares
<details>
  <summary> Answer </summary>

  The threshold number of key shares
  Shamir’s Secret Sharing is a cryptographic algorithm that allows a secret to be split into multiple parts, called key shares, such that a certain number of key shares are required to reconstruct the secret. The number of key shares and the threshold number are configurable parameters that depend on the desired level of security and availability. Vault uses Shamir’s Secret Sharing to protect its master key, which is used to encrypt and decrypt the data encryption key that secures the Vault data. When Vault is initialized, it generates a master key and splits it into a configured number of key shares, which are then distributed to trusted operators. To unseal Vault, the threshold number of key shares must be provided to reconstruct the master key and decrypt the data encryption key. This process ensures that no single operator can access the Vault data without the cooperation of other key holders.

  Reference:
  https://developer.hashicorp.com/vault/docs/concepts/seal4,
  https://developer.hashicorp.com/vault/docs/commands/operator/init5,
  https://developer.hashicorp.com/vault/docs/commands/operator/unseal6

</details>