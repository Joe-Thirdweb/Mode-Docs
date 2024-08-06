# 01/08/2024 Bridge Upgrade Fund Rescue

**Bridge Upgrade to Rescue Stuck Funds**

On 1st August 2024 Mode ran a planned upgrade to rescue 4880 ETH in funds sent to an incorrect L2 proxy address by Etherfi. \
\
[**Full post-mortem on the EtherFi github here.** ](https://github.com/etherfi-protocol/postmortems/?tab=readme-ov-file)\
[**Bridge upgrade transaction to rescue funds.** ](https://etherscan.io/tx/0x9154d2b581e84b15615b4a857476af9fa6b682622d6e30e7c28bae6331a5fe39)

**Overview**\
\
On May 8, 2024, Etherfi generated a proof using an incorrect messenger for withdrawals on the Mode Bridge \
\
Shortly after the event, the EtherFi team reached out to Mode and an initial proposal was made by Nethermind ([link](https://github.com/etherfi-protocol/postmortems/blob/master/1715209000-l2-l1-sync-misconfiguration/NM0243-ETHERFI-REPORT.pdf)) to rescue the funds. \
\
After multiple security reviews, Mode executed an upgrade to rescue the stuck funds and return them to Etherfi. The upgrade was successfully completed with minimal disruption to Mode Network. \
&#x20;\
**Why was this rescue not disclosed in advance?**&#x20;

There were concerns of making this rescue public due to the risk of the message not being finalized or a malicious actor sending a proof. After multiple security reviews we found a safer solution which bypassed these risks and executed this.&#x20;

**For further details see the** [**full post-mortem on the EtherFi github here.** ](https://github.com/etherfi-protocol/postmortems/?tab=readme-ov-file)

\










On May 8, 2024, we generated proofs for withdrawals from various L2s to Ethereum Mainnet. During this process we ran into an error generating the withdrawal proofs for the Mode network. We proceeded to reach out to the Mode team to investigate this issue, and later came to the conclusion that our contracts were configured with the incorrect messenger contract address, rendering the funds stuck on the bridge.

The misconfigured messenger address can be seen in the `initialize` call [shortly after](https://explorer.mode.network/tx/0x7615241a93e8d64560a8f9169ff82c048b53ae65d6bf3caee0dd9e85a0f8878a) contract creation, where it was initially set to [0xc0d3c0d3c0d3c0d3c0d3c0d3c0d3c0d3c0d30007](https://explorer.mode.network/address/0xc0d3c0d3c0d3c0d3c0d3c0d3c0d3c0d3c0d30007).

On May 10, we believe we found the most minimal change necessary that will allow the withdrawal to successfully move through the remaining states and into the rest of the ether.fi protocol on Ethereum mainnet.

We consulted Nethermind on our proposed solution to recover the locked funds, which was then proposed to the Mode team as a solution. Read more in the consultation report [attached here](https://github.com/etherfi-protocol/postmortems/blob/master/1715209000-l2-l1-sync-misconfiguration/NM0243-ETHERFI-REPORT.pdf).

As part of the sensitive operations involved in recovering the funds, the proof submission was crucial to execute at exactly the right time. Since it is permission-less, it prevented us from revealing anything until the recovery was successfully executed.

On Aug 1, 2024, the Mode team approved to [rescue the funds](https://etherscan.io/tx/0x9154d2b581e84b15615b4a857476af9fa6b682622d6e30e7c28bae6331a5fe39) directly by withdrawing it from Mode’s Portal contract.

We learnt important lessons during this process and document the improvements we've made to our process. Read more about that in our [future preventative measures](https://github.com/etherfi-protocol/postmortems/blob/master/1715209000-l2-l1-sync-misconfiguration/prevention.md).





















{% embed url="https://github.com/etherfi-protocol/postmortems/?tab=readme-ov-file" %}