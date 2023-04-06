# solana-encryption-lib
A Proof of concept for on chain RSA management

Let's say you are editing a Text Document and want to share it to a friend, without making your document visible to the public.
You will probably want to use some kind of access control to restrict people viewing or editing your data.
This is where this library comes in.

The Encryption Lib is structured in two parts, a Typescript SDK and a Rust Crate. Let's see how you can interact with them.

Let's look at an example where a business issued an invoice to a buyer. This invoice will be stored on a public Filesystem in an encrypted way. The buyer then wants to share this invoice to his tax consultant.
First of all, this is how the code on the Business side should look like:
```ts
// Create a symmetric encryption key
const encryption = new Encryption()

// Create the three parties Keypairs (this is all done in one file but can be done in multiple)
const business = Keypair.generate()

// The business first encrypts and uploads their file
encrypt("invoice.pdf, encryption)

// The business creates the on chain encryption and gives write access control to the buyer
const buyer = new PublicKey("...")
const reference = // any reference to find the account in the future, can be Mint ID, file link, person name etc..
await createBox(encryption, accounts: { buyer, access_control: 4 }, reference, business)
```
Then, the buyer can give access to his tax consultant.
```ts
const buyer = Keypair
const consultant = new PublicKey("...")
// Because the buyer has write access (access_control : 4) he can add new users to the file.
await addToBox(accounts: { consultant, access_control: 2 }, reference, buyer) // (read access control)
```
Last off, the tax consultant will then decrypt the file
```ts
const consultant = Keypair
const reference = // any
const encryption = getEncryption(reference, consultant)
decrypt("invoice.pdf", encryption)
```
The Rust Crate can be used to automatically add or change access controls directly from your program.

To integrate this feature into your wallet, follow the [Integration Docs]()

