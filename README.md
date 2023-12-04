
# Olympus RBS 2.0 contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
mainnet
___

### Q: Which ERC20 tokens do you expect will interact with the smart contracts? 
DAI, sDAI, wETH, veFXS, FXS, OHM
___

### Q: Which ERC721 tokens do you expect will interact with the smart contracts? 
UNI-V3
___

### Q: Do you plan to support ERC1155?
No
___

### Q: Which ERC777 tokens do you expect will interact with the smart contracts? 
None
___

### Q: Are there any FEE-ON-TRANSFER tokens interacting with the smart contracts?

No
___

### Q: Are there any REBASING tokens interacting with the smart contracts?

No
___

### Q: Are the admins of the protocols your contracts integrate with (if any) TRUSTED or RESTRICTED?
TRUSTED
___

### Q: Is the admin/owner of the protocol/contracts TRUSTED or RESTRICTED?
TRUSTED
___

### Q: Are there any additional protocol roles? If yes, please explain in detail:
The Bophades system includes support for permissions and roles, which the in-scope contracts utilise.

In the in-scope modules (TRSRY, PRICE and SPPLY), some functions are gated with the permissioned modifier. These functions can only be called by a policy that has explicitly listed that function selector as a dependency in the policy's requestPermissions() function. Policies can only be installed and activated by the system owner (at this time, the OlympusDAO multi-sig). These permissioned functions are largely configuration/admin-related, and other functions (such as view functions) are un-gated.
___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
No
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
The PRICE submodules that use an on-chain method to access the reserves of a liquidity pool or positions are susceptible to sandwich attacks and multi-block manipulation
    - Assets in PRICEv2 can be configured to track the moving average of an asset price in order to mitigate this risk
    - Assets in PRICEv2 can be configured with multiple price feeds and a reconciliation strategy (e.g. average, median, average if a deviation is present) in order to mitigate this risk
    - Where possible, PRICE submodules will check for re-entrancy in the source (e.g. liquidity pool). This has been implemented in the BalancerPoolTokenPrice and Uniswap submodule.

The SPPLY submodules that use an on-chain method to access the reserves of a liquidity pool or positions are susceptible to sandwich attacks and multi-block manipulation
    - Where possible, downstream consumers of the data need to conduct sanity-checks.
    - Where possible, SPPLY submodules will check for re-entrancy in the source (e.g. liquidity pool). This has been implemented in the AuraBalancerSupply submodule.
    - To guard against multi-block manipulation, where possible, SPPLY submodules will compare the implied price from reserves against the price from TWAP. This has been implemented in the BunniSupply submodule.
___

### Q: Please provide links to previous audits (if any).
https://docs.olympusdao.finance/main/security/audits/
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?
No
___

### Q: In case of external protocol integrations, are the risks of external contracts pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality.
Such risks would not be acceptable, as the TRSRY, PRICE and SPPLY modules collectively track and manage the Olympus protocol treasury and token supply. Emergency withdrawals would affect the treasury valuation and/or token supply, and hence higher-level metrics (such as liquid backing per backed OHM), which would affect other components of the protocol.
___

### Q: Do you expect to use any of the following tokens with non-standard behaviour with the smart contracts?
DAI, USDC
___

### Q: Add links to relevant protocol resources
https://docs.olympusdao.finance/

Please also see relevant audit documentation in the repository:

- audit/23-11_price-v2/README.md
- audit/23-11_spply_trsry/README.md
___




# Audit scope