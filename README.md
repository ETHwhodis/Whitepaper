# Whitepaper

## 1 Problem Statement

There are significant issues in data privacy including but not limited to political, financial and social.  While blockchains facilitate human coordination, they are susceptible to mass surveillance by bad actors.  According to Vitalik Buterin, "Currently there are large privacy problems in the ethereum ecosystem. The default behavior is to do everything through a single account, which allows all of a user’s activities to be publicly linked to each other. It seems like this can be improved by using multiple addresses, but not really: the transactions you make to send ETH to those addresses themselves reveal the link between them...  This greatly hinders adoption of many applications..." 

The first major privacy solution for ethereum is Tornado.Cash.  It uses zero knowledge proofs and smart contracts to break the on-chain link of an ethereum transaction.  No doubt a breakthrough in cryptographic tools yet it may be possible to link a transaction ownership based on time and amount of deposits since a user initially deposits into the Tornado smart contract.  As stated by Tornado.Cash,

#### (A) If a deposit and a withdrawal are right next to each other, it is very likely that they belong to the same person. We recommend waiting until at least a few deposits are made after yours before withdrawing the note.

#### (B) If there is a batch of deposits from one address, and then a batch of the same size of withdrawals to a single address, they are very likely connected. If you need to make multiple withdrawals, try to spread them out and withdraw to addresses not linked with each other.

#### (C) Wait until some time has passed after your deposit. Even if there are multiple deposits after yours they all might be made by the same person that is trying to spam deposits and make users falsely believe that there is large anonymity set when in fact it is lower (also known as a Sybil attack). We recommend waiting at least 24 hours to make sure that there were deposits made by multiple people during that time. Check the instance statistics when using it.

What is needed is an easy to use, 100% open source privacy solution that quantifies privacy for ethereum transactions, all within a user's work flow.

Building on Tornado.Cash, WHODIS.ETH quantifies a user's privacy based in time and amount of deposits through a new standard: AnonymityScore. In addition, WHODIS.ETH provides a user friendly interface to automatically generate provably unused, non-custodial ethereum addresses locally, within the browser as hosted on the Interplanetary File System (IPFS). 

## 2 Design Approach

The end state requirement for a provably anonymous on-chain ETH transaction is that the sender and receiver of the ETH cannot be linked.  We define provable anonymity as dependent on the following variables:

(1) Deposits;
(2) Time; and
(3) Receiver Address.

WHODIS.ETH uses the Tornado smart contract and circuit, therefore providing a predefined deposit anonymity set and audited codebase.  The more deposits that enter the Tornado smart contract (that do not withdraw), the greater the anonymity set becomes - unless it is spammed by desposits from the same account or linked accounts.  

#### 2.1 Anonymity Score

Key Assumption: The more time that elapses between deposits, the more likely it is that such deposits are from separate users.  Therefore, the more deposits that accumulate over a longer period of time, the greater the anonymity set becomes.  We quantify this as AnonymityScore:

![image](https://user-images.githubusercontent.com/59490498/76040140-9282f380-5f1c-11ea-9ac0-f64b6efbd3d4.png)
The AnonymityScore is a function of the Anonymity_Level chosen by a user.  

#### 2.2 Anonymity_Level 


The Anonymity_Level can be can be Low, Medium or High.

The variables for the Anonymity_Level are total_time, elapsed_time and depositsSince.

total_time for Anonymity_Level of:

Low = 6 hours

Medium = 12 hours

High = 24 hours.

elapsed_time = amount of time since user's deposit.

depositsSince = the number of deposits during the elapsed_time (aka since user deposited).  

The following formula to determine your AnonymityScore based on the Anonymity_Level you chose.

((elapsed_time * total_time) * 2)+(depositsSince * 20)

Deposits are valued by 10x more than time (deposits are multiplied by 20 where as time is multiplied by 2) because the amount of deposits made after the user's deposit enable the user to hide in the smart contract amongst the other depositors.  Because of the spam attack as stated above, as long as such deposits take place over an extended period of time the user can quantifiably anonymize their data transaction.  

This formula is illustrated through the following examples for the Anonymity_Level of:

#### Low

![image](https://user-images.githubusercontent.com/59490498/76040388-29e84680-5f1d-11ea-9398-82d77e23cb5e.png)

#### Medium

![image](https://user-images.githubusercontent.com/59490498/76040404-37053580-5f1d-11ea-9ce8-722f350e9f0d.png)

#### High

![image](https://user-images.githubusercontent.com/59490498/76040417-43898e00-5f1d-11ea-951b-aaab09aa78ff.png)

The AnonymityScore increases for each Anonymity_Level the more time that elapses and the more deposits that are made. 

![image](https://user-images.githubusercontent.com/59490498/76040114-7da66000-5f1c-11ea-821e-6c412a04e957.png)

Briefly, when you get less deposits, it will take more time than estimated to reach the chosen Anonymity_Level.  

![image](https://user-images.githubusercontent.com/59490498/76040092-67000900-5f1c-11ea-8c66-8d71761e48d5.png)

When you get more deposits, it will take less time to reach the chosen Anonymity_Level.  

![image](https://user-images.githubusercontent.com/59490498/76040040-37e99780-5f1c-11ea-959e-67add788029d.png)

There is an option to "withdraw now" - before the Anonymity_Level progress reaches 100%.  If the "withdraw now" button is used, the below prompt will come up - beware that your privacy cannot be 100% guaranteed.  

![image](https://user-images.githubusercontent.com/59490498/76040880-91eb5c80-5f1e-11ea-9ada-f33a5f41b284.png)

#### 2.3 zkSnark Work Flows

In Tornado.Cash, in order to deposit into the smart contract, the user must generate a "Note" aka secret to an unspent commitment from the smart contract's list of deposits.  The Note contains the data that can be used to provably link a users deposit with withdrawal.  zkSnark technology allows this to happen without revealing which deposit corresponds to this secret and therefore the withdrawal.  Tornado.Cash requires backing up the Note's data and then inputting the Note before withdrawal (this allows the smart contract to check the proof and transfer the deposited funds to the address specified for withdrawal).  If the Note's data is lost or stolen, the ETH is gone forever.  The Note is essentially the private key.  This approach has resulted in approximately 4 million Bitcoin and a corresponding amount of ETH, ERC-20 tokens and many more cryptocurrencies being lost forever.

A key goal of WHODIS.ETH is for anyone with internet access to easily practice good data hygiene on the ethereum blockchain - all within their workflow and without risk of losing their funds.  We provide the ability for the Note to automatically be generated and stored locally (like as in Metamask), never on the browser, such that the user does not have to consider backing up the Note or losing his funds because of losing the Note or having it stolen.  Notably, the user is provided the option to view and backup the Note, but this is unnecessary and may provide significant attack vectors as previously stated.

#### 2.4 Receiver Address (Withdraw)

WHODIS.ETH provides the unique ability to generate a new, unused, non-custodial address locally, within the browser.  It is highly recommended that the user takes advantadge of this feature.  The other option is for the user to generate a new ethereum address on their own (e.g. mycrypto, mew, metamask, rainbow, trust wallet, etc.).  If the user does not generate a new, unused address and instead withdraws the ETH to a previously used ethereum address, privacy guarantees are compromised.

After a user has achieved a 100% score, to ensure privacy, the ETH can either stay in the smart contract (builds the Tornado.Cash anonymity set) or the ETH MUST be withdrawn through a relayer to an unused ethereum address.  If a relayer is not used, your privacy cannot be 100% guaranteed.  WHODIS.ETH provides the option to use a relayer.

## 3 Comparisons + Future Work

In addition to Tornado.Cash, MicroMix (Kovan Testnet) also uses zkSnarks to mix transaction data.  The UI hides the anonymity set and encourages users to wait until midnight UTC to withdraw.  It is important for the user to know how many depositers, over what period of time, deposited into the smart contract - the anonymity set.  Without this information the user cannot be assured of his privacy guarantees.  In addition, the withdrawal at midnight UTC likely poses the same issue as Tornado describes in 1(A) above.  A more ideal improvement would be the ability to programmably "schedule" those transactions to be submitted by the relayer at midnight UTC (similar to a time lock-unlock).  This way it is unknown who withdrew what, because it all happened at the same time.    New potential attack vectors should be considered with this approach.

Upon eliminating the attack vectors of the MicroMix approach, it will be possible to create a defi * privacy operation, akin to PoolTogether.  Notably, users are encouraged to leave deposits in the smart contract as long as possible to grow the anonymity set.  Deposits would be required to enter by a certain time and withdrawals would have to happen within a certain time period (for payouts) - incentivizing non-withdrawal.  An anonymous version of PoolTogether can result various political, financial and social solutions. This is early research.  All attack vectors must be considered.  

## Acknowledgements

Made possible discussion barryWhiteHat, Kobi Gurkan, Wei Kie Koh, Roman Semenov and an anonymous ethereum developer.  Grateful for research provided by Tornado.Cash team, the Semaphore team, the Zcash team and the Ethereum Foundation.

## License

GNU General Public License v3.0

## References

Alexey Pertsev, Roman Semenov, Roman Storm.  Tornado Cash Privacy Solution, Version 1.4 (December 2019).

Wie Jie Koh.  MicroMix, https://github.com/weijiekoh/mixer (2019).

barryWhiteHat.  Solving optimization problems using snarks , ethereum and incentives, https://ethresear.ch/t/solving-optimization-problems-using-snarks-ethereum-and-incentives/4904 (January 2019).

