# Advance_solidity-homework

1st: Code the file PupperCoinCrowdsale.sol leveraging the code from Crowdsale.sol

2nd: Attemept to compile and Deploy the code for PupperCoinCrowdsale, initially in LocalHost to check that everything is working fine

3rd: Attempt to add the token created above to a wallet (in MetaMask) and then send some tokens among addresses, checking that transfers are working fine from contract in Remix:

4th: Recreate the whole process in Kovan-Testnet network:


pragma solidity ^0.5.0;

import "./PupperCoin.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/Crowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/emission/MintedCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/validation/CappedCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/validation/TimedCrowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/distribution/RefundablePostDeliveryCrowdsale.sol";

// @TODO: Inherit the crowdsale contracts
contract PupperCoinSale is Crowdsale, MintedCrowdsale, CappedCrowdsale, TimedCrowdsale,RefundablePostDeliveryCrowdsale {

    constructor(
       uint256 rate,
       address payable wallet,
       PupperCoin token,
       uint256 goal,
       uint256 open,
       uint256 close
    )

       Crowdsale(rate, wallet, token)
       MintedCrowdsale()
       CappedCrowdsale(goal)
       TimedCrowdsale(open, close)
       PostDeliveryCrowdsale()
       RefundableCrowdsale(goal)
       public
    {
       // constructor can stay empty
    }
}

contract PupperCoinSaleDeployer {

    address public token_sale_address;
    address public token_address;

    constructor(
        // @TODO: Fill in the constructor parameters!
        string memory name,
        string memory symbol,
        address payable wallet,
        uint goal
    )
        public
    {
        // @TODO: create the PupperCoin and keep its address handy
        PupperCoin token = new PupperCoin(name, symbpl, 0);
        token_address = address(token);

        // @TODO: create the PupperCoinSale and tell it about the token, set the goal, and set the open and close times to now and now + 24 weeks.
        PupperCoinSale pupper_sale = new PupperCoinSale(1, wallet, token, goal, now, now + 24 weeks)
        token_sale_address = address(pupper_sale);
        
        // make the PupperCoinSale contract a minter, then have the PupperCoinSaleDeployer renounce its minter role
        token.addMinter(token_sale_address);
        token.renounceMinter();
    }
}
