# Variant 1: E-coupon distribution protocol via EC-APP
The explanation of Variant 1 of the EC-APP protocol is omitted from the paper due to space constraints. We explain the protocol for e-coupon distribution via EC-APP.  Here, we give the communication flow and a step-by-step description.
## Communication Flow
![Communication flow of Variant 1](https://github.com/user-attachments/assets/688b1454-e1c0-4374-894f-b959b4912eff)
## Step-by-Step Explanation
The Issuer starts by generating the first and second halves of the code inside the e-coupon, which are distributed to the users in two halves: the first half $(FH)$ and the second half $(SH)$. The Issuer then computes the complete e-coupon codes such as $EC=f(FH,SH)$, where $f$ is a one-way function, e.g., a cryptographic hash function or a key derivation function. 

The Issuer prepares e-coupon codes, for each type of e-coupon, and applies the hash function to them: $f(EC)$. The Issuer signs the list of hashed e-coupon codes and sends the signed list to the Shop.

The Issuer generates two types of nonces, NoncePSM and NoncePSS, and sends them to the corresponding PSs. These nonces serve as a proof to the Issuer that Alice has been in proximity of the PSs during a specific time period. Therefore, the Issuer updates the nonces regularly and informs the PSs about the change. Note that `Alice' is used as a generic name for the customer, and the Issuer does not learn the customer's actual identity. 

1) When Alice is near the Mall, PS-Mall detects her and sends a notification to the EC-APP on Alice's device. This notification includes information about e-coupons, e.g., which Shop is offering what kind of discount, the expiration time of each e-coupon, and the relevant nonce (NoncePSM). 

2) If Alice opens the notification and accepts an e-coupon, the NoncePSM is sent through the secure channel of EC-APP to the Issuer. 

3) The Issuer responds with the first half of the e-coupon code (FH).

4) When Alice goes to the Shop, PS-Shop detects that she is nearby and sends a notification, including its nonce (NoncePSS). 

5) FH and NoncePSS are sent to the Issuer via EC-APP. 

6) Issuer responds with the second half (SH), corresponding to the FH.

7) After receiving both FH and SH, the e-coupon code is computed in the EC-APP, i.e., $EC=f(FH,SH)$, and it appears as a QR code on Alice's device and remains until it expires. 

8) When Alice wants to use the e-coupon, she presents the QR code to the Shop. 

9) Then, the Shop computes the hash of the e-coupon code and checks if it matches any of the stored e-coupons. At this point, if the Shop allows the double use of the e-coupon, the list of the Shop remains unchanged. Otherwise, the Shop removes the hashed value of the used e-coupon from the stored list or marks it as used.  
