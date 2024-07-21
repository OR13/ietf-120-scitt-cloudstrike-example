# IETF 120 Hackathon Project

Objective: Show how SCITT and Transparency Services could be used by Computer Security Incident Response (CSIRT) in incidents similiar to July 19th, in the future.

🚧 I have not tested any of this.

🚧 This is just an example, developed in the IETF 120 Hackathon.

See the fixer script here for more details: [https://github.com/alexdelifer/crowdstrike-fixer](https://github.com/alexdelifer/crowdstrike-fixer).

CSIRT team arrives at a data center impacted by the incident.

The building security team wants to confirm the CSIRT team is in possession of recovery media that is authorized for use in this data center.

The building security team checks that the bootable media and fixer media are signed by certificates that are trusted by the CSIRT team.

Next the building security check that the signed recovery script are notarized in the data center notary. 

The following bash script example sketches some of the operations that the building security team might perform in order to establish that the incident response team is in possession of authorized recovery tools.

```bash
# We assume the bulding security team has certificates for the CSIRT and Data Center Notary,
# and that scitt-verify script has access to these certificates

# download the script
curl -fLs https://raw.githubusercontent.com/alexdelifer/crowdstrike-fixer/main/autorun > autorun

# hash the script
sha256sum autorun
# 80a4cce35fbd953a497b551756d59ea7f071b77cb39a044f4ff0ff08af1aa19e  autorun

# Then the building security team retrieves the executables from the recover media
# And they confirm the hashes and signatures are matching and acceptable...

# verify the transparency of the recovery script
# scitt-verify ./autorun.transparency.cbor ./autorun

# Data Center Notary Receipt Signature Verified with Certificate Thumbprint:
# ✅ 52bdc3ed42f3a233e36312d85670625003df560817dfd3fcde071471ca309149 

# CSIRT Signature Verified with Certificate Thumbprint:
# ✅ 72137c02c2d762edb2ed88f225a605404093df02185eaf7640ed25c1a423c3c0 

# Payload Verified:
# ✅ 80a4cce35fbd953a497b551756d59ea7f071b77cb39a044f4ff0ff08af1aa19e

# Decoded CBOR Extended Diagnostic Notation for Verified Script Transparency
# 18([
#   <<{
#     / Certificate Hash with SHA256  / 34: [ -16, h'830403...92e7']
#     / Script Hash signed with ES256 / 1: -7, 
#     / Script Content Type       / -6802: "text/x-shellscript", 
#     / Script Location           / -6801: "https://raw.githubusercontent.com/alexdelifer/crowdstrike-fixer/main/autorun",
#     / Script hashed with SHA256 / -6800: -16
#   }>>, 
#   {
#     / Signature Receipts / 394: [ 
#       <<
#       18([
#         <<{
#           / Certificate Hash with SHA256  / 34: [ -16, h'0b317603a2975...6abe2ae194']
#           / Receipt signed with ES256                   / 1: -7, 
#           / Receipt stored in SHA256 Binary Merkle Tree / 395: 1
#         }>>, 
#         {
#           / Receipt has proofs              / 396: {
#           / Inclusion in Binary Merkle Tree / -1: [
#               <<[
#                 / Tree Size                     / 4, 
#                 / Index of Signature            / 3, 
#                 / Inclusion Path to Merkle Root / 
#                 [
#                   h'0b317603a297598bcb4c109248370bf97f064ad5ce72540c51ef074cf65d1ea6', 
#                   h'2266e9ddea46d3982ec341277a629fd2293996d5b6abe2ae19431821f94e92e7'
#                 ]
#               ]>>
#             ]
#           }
#         }, 
#         / Merkle Root / null, / Reconstructed at Verification Time /
#         / Signature / h'a59d6c923fad8...6860ab538b3a4816a76067b86e7a099'
#       ])
#       >>
#     ]
#   }, 
#   / Payload is Hash of Script / h'80a4cce35fbd953a497b551756d59ea7f071b77cb39a044f4ff0ff08af1aa19e',
#   / Signature      / h'4fc9d761d8752e12...bcb1ca59b77f51fa0061c8d'
# ])
```
