# BEER Coin

The BEER Coin is a new, revolutionary, mineable type of token presented exclusively on [Polygon (MATIC)](https://polygon.technology) network.

The coin features have been carefully designed with the following goals in mind:

* Fun to own
* Easy and exciting to mine
* Fair and balanced distribution
* Beneficial to save and accumulate
* Inspiring further innovation

## Introduction

The coin's smart contract presents miners a list of challenges of various difficulties that they can try to solve using the mining page, GoMiner software, or their own mining technology to find the solutions as soon as possible. Up to 100 regular and up to 100 premium challenges can presented at the same time. Each challenge can be solved and claimed by several miners up until all coins allocated for each challenge are claimed. When all allocated coins are rewarded, the challenge expires and new challenge with highest difficulty will be added to the bottom of the list and difficulty of all previous challenges will be lowered by 2 difficulty points. That way it is guaranteed that all challenges will eventually be solved by miners in reasonable amount of time. The frequency of new challenges is limited though to 1 per 100 blocks or 1 per approximately 210 second (3.5 minutes).

## Challenges

Smart contract can create a new challenge in average every 100 POlygon blocks. We assume each Polygon blocks takes approximately 2.1 seconds so a new challenge can be added approximately every 3.5 minutes. The challenges are actually hashes of the most recent blocks deterministally selected by the smart contract at the time of their challenge creation. The goal of the miners is to combine this hash with the unique address of their wallet and find a nonce that will produce Keccak-256 hash withing certain range limited by difficulty. By combining the block-hash with individual wallet address the miners are looking for unique solutions specific for their wallets. That way it's not possible to use one solution for claiming multiple rewards. The challenges with special block-hashes that start with one or more lucky digits number 8 are classified as premium challenges, that have higher rewards and higher number of mienrs can claim them. Below is a summary of all types of challenges and their properties.

|Level|Challenge|BlockHash Criteria|Reward Coins|Block Frequency|Time Frequency|Challenges Per Year|Coins per Year|
|:---:|---|---|:---:|---|---|---:|---:|
|0|Regular|Does not start with 0x8|10 x 1 coin|1 per 106.67 blocks|~3.75 minutes|140,785|1,407,857|
|1|Premium|Starts with 0x8|20 x 5 coins|1 per 1,706.67 blocks|~1 hour|8,799|879,911|
|2|Premium|Starts with 0x88|50 x 20 coins|1 per 27,306.67 blocks|~16 hours|549|549,944|
|3|Premium|Starts with 0x888|100 x 100 coins|1 per 436,906.67 blocks|~10 days|34|343,715|
|4|Premium|Starts with 0x8888|200 x 500 coins|1 per 6,990,506.67 blocks|~6 months|2|214,822|
|5|Premium|Starts with 0x88888|500 x 2000 coins|1 per 104,857,600 blocks|~7 years|0.14|143,215|
TOTAL||||1 per 100 blocks in average||150,171|3,539,464|

## Solved Difficulty

The smart contract verifies if the miner solved the required difficulty as below. The keccak256 hash is calculated on blob produced by combining bits pf block number, block hash, and wallet address, concatenated with the solution found by the miner. If the top N bits in the resulting hash are all 0, then the smart contract will acknowledge the miners solution bits are solving the required difficulty of N.

```
    function verifySolution(uint64 blockNumber, uint256 solution) public view whenNotPaused returns (uint16 solvedDifficulty) {
        (Challenge memory ch, bool premium, uint8 generalDifficulty) = _retrieveChallenge(blockNumber);

        bytes32 digest = keccak256(abi.encodePacked(uint256(ch.blockNumber) ^ uint256(ch.blockHash) ^ uint256(msg.sender), solution));

        solvedDifficulty = 256 - _findHsb(uint256(digest));
    }
```

## Difficulty

Difficulty is expressed as a simple integer number between 20 and 218. The difficulty 20 is the easiest one and the difficulty 218 is the most hard. In average the miners will have to generate 2 powered to difficulty number of hashes in order to solve the challenge. The table below shows an average time needed to solve a challenge of particular difficulty using a web browser (hash rate 10K hashes / second), GO Miner tool on PC (5M hashes / second), and on custom mining solution using a GPU or FPGA (5G hashes / second)

|Difficulty|Hash Count|Web Browser||GO Miner||Custom/GPU||
|---|---:|---:|---|---:|---|---:|---|
|20|1048576|1.75|min|0.21|s|0.00|s|
|22|4194304|6.99|min|0.84|s|0.00|s|
|24|16777216|27.96|min|3.36|s|0.00|s|
|26|67108864|1.86|hr|13.42|s|0.01|s|
|28|268435456|7.46|hr|53.69|s|0.05|s|
|30|1073741824|1.24|days|3.58|min|0.21|s|
|32|4294967296|4.97|days|14.32|min|0.86|s|
|34|17179869184|19.88|days|57.27|min|3.44|s|
|36|68719476736|79.54|days|3.82|hr|13.74|s|
|38|2.74878E+11|318.15|days|15.27|hr|54.98|s|
|40|1.09951E+12|3.48|years|2.55|days|3.67|min|
|42|4.39805E+12|13.94|years|10.18|days|14.66|min|
|44|1.75922E+13|55.75|years|40.72|days|58.64|min|
|46|7.03687E+13|222.99|years|162.89|days|3.91|hr|
|48|2.81475E+14|891.94|years|1.78|years|15.64|hr|
|50|1.1259E+15|3,567.76|years|7.14|years|2.61|days|
|52|4.5036E+15|14,271.05|years|28.54|years|10.42|days|
|54|1.80144E+16|57,084.18|years|114.17|years|41.70|days|
|56|7.20576E+16|228,336.74|years|456.67|years|166.80|days|
|58|2.8823E+17|913,346.95|years|1,826.69|years|1.83|years|
|60|1.15292E+18|3,653,387.79|years|7,306.78|years|7.31|years|
|62|4.61169E+18|14,613,551.15|years|29,227.10|years|29.23|years|
|64|1.84467E+19|58,454,204.61|years|116,908.41|years|116.91|years|
|66|7.3787E+19|233,816,818.44|years|467,633.64|years|467.63|years|
|68|2.95148E+20|935,267,273.75|years|1,870,534.55|years|1,870.53|years|
|70|1.18059E+21|3,741,069,094.98|years|7,482,138.19|years|7,482.14|years|
|72|4.72237E+21|14,964,276,379.92|years|29,928,552.76|years|29,928.55|years|
|74|1.88895E+22|59,857,105,519.68|years|119,714,211.04|years|119,714.21|years|
|76|7.55579E+22|239,428,422,078.72|years|478,856,844.16|years|478,856.84|years|
|78|3.02231E+23|957,713,688,314.88|years|1,915,427,376.63|years|1,915,427.38|years|
|80|1.20893E+24|3,830,854,753,259.53|years|7,661,709,506.52|years|7,661,709.51|years|
|82|4.8357E+24|15,323,419,013,038.10|years|30,646,838,026.08|years|30,646,838.03|years|
|84|1.93428E+25|61,293,676,052,152.50|years|122,587,352,104.31|years|122,587,352.10|years|
|86|7.73713E+25|245,174,704,208,610.00|years|490,349,408,417.22|years|490,349,408.42|years|
|88|3.09485E+26|980,698,816,834,439.00|years|1,961,397,633,668.88|years|1,961,397,633.67|years|
|90|1.23794E+27|3,922,795,267,337,760.00|years|7,845,590,534,675.51|years|7,845,590,534.68|years|
|92|4.95176E+27|15,691,181,069,351,000.00|years|31,382,362,138,702.10|years|31,382,362,138.70|years|
|94|1.9807E+28|62,764,724,277,404,100.00|years|125,529,448,554,808.00|years|125,529,448,554.81|years|
|96|7.92282E+28|251,058,897,109,617,000.00|years|502,117,794,219,233.00|years|502,117,794,219.23|years|
|98|3.16913E+29|1,004,235,588,438,470,000.00|years|2,008,471,176,876,930.00|years|2,008,471,176,876.93|years|
|100|1.26765E+30|4,016,942,353,753,860,000.00|years|8,033,884,707,507,730.00|years|8,033,884,707,507.73|years|
|102|5.0706E+30|16,067,769,415,015,500,000.00|years|32,135,538,830,030,900.00|years|32,135,538,830,030.90|years|
|104|2.02824E+31|64,271,077,660,061,800,000.00|years|128,542,155,320,124,000.00|years|128,542,155,320,124.00|years|
|106|8.11296E+31|257,084,310,640,247,000,000.00|years|514,168,621,280,494,000.00|years|514,168,621,280,495.00|years|
|108|3.24519E+32|1,028,337,242,560,990,000,000.00|years|2,056,674,485,121,980,000.00|years|2,056,674,485,121,980.00|years|
|110|1.29807E+33|4,113,348,970,243,960,000,000.00|years|8,226,697,940,487,910,000.00|years|8,226,697,940,487,910.00|years|

As we see, even though difficulty is expressed as a simple number, it is practically impossible to solve challenges with higher difficulty number using current technologies.
For example a custom solution with 5G/s hash rate would need 8 billion years to solve the difficulty 100. That is longer than the lifetime of the sun.

## Difficulty Discounts

In order to make the mining process more fair and balanced, the smart contract will grant discounted difficulty to specific miners using predefined rules based on their wallet address and their registered balance.

## Registered Ballance

In order to incentivise saving and accumulation of BERR coins, the smart will grant some discounts to miners based on their BEER token balance. In order to have fair rules, it is not desirable to apply the current balance at the time of claiming the reward, because miners could easily borrow or move coins to their wallets right before claiming. It is only fair to apply the balance maintained during the whole time of mining any particlar challenge. From technical perspective it is not practical to keep the full history of the wallet balances in the blockchain. Therefore we came out with the concept of "registered balance", which will be managed by the smart contract as follows.

* at the smart contract creation, the registered balance of any wallet is 0
* when wallet receives and mines any BEER coins, the owner can register their non zero balance with the smart contract from the mining web page
* the registered balance will be applicanle to future challenges created after the registration
* the smart contract will maintain up to 3 registerd balances applicable at time intervals between the registration
* the latest registrered balance will be automatically by the smart contract adjusted downwards, in case any trasfer decreases the current balance below the latest registered balance
* in case any coin trasfer increases the current balance, the smart contract will NOT adjust the latest registered balance

Based on the rules above the miners might consider registering their balance in following cases:

* after increasing a balance in their wallet to make the smart contract apply the higher registered balance
* before selling or tranferring out the coins from the wallet, to protect the latest registered amount for the period since it was registered

## Wallet Difficulty Discount

The address discount determined based on how "similar" the wallet address is to the challenge's block-hash. The contract will compare lowest significant bits of the block-hash with the lowest significant bits of the wallet's address. The formula is defined as follows:

```
    n = lsb(blockHash ^ address)
    if(balance >= 1.0) {
        if(n >= 20) {
            n = 30
        } else if(n >= 10) {
            n = 20 + (n - 10)
        } else {
            n = 2 * n
        }
    } else {
        if(n > 10) {
            n = 10
        }
    }
```

The wallet difficulty discount is then determined as follows:

|N|0 <= RB < 1|1 <= RB|
|---|:---:|:---:|
|(matching bits)|(wallet discount)|(wallet discount)|
|0|0|0|
|1|1|2|
|2|2|4|
|3|3|6|
|4|4|8|
|5|5|10|
|6|6|12|
|7|7|14|
|8|8|16|
|9|9|18|
|10|10|20|
|11|10|21|
|12|10|22|
|13|10|23|
|14|10|24|
|15|10|25|
|16|10|26|
|17|10|27|
|18|10|28|
|19|10|29|
|20+|10|30|

* N - number of matching lowest significant bits between the wallet address and the block-hash
* RB - registered balance, number of BEER coins in the wallet

The maximum wallet discount possible is 30. Please note the wallet discount is capped at 10 on wallets with no or very little balance, to limit ability of miners to generate huge number of wallets for the sole purpose of matching the the challenges.

## Balance Discount

The wallet discount can further be amplified by a balance discount granted by the smart contract to miners who registered their BEER coin balances. The balance discount is granted as follows:

|Registered Balance|Balance Discount|
|---:|:---:|
|0.000|0|
|0.001|1|
|0.002|2|
|0.005|3|
|0.010|4|
|0.020|5|
|0.050|6|
|0.100|7|
|0.200|8|
|0.500|9|
|1|10|
|2|12|
|3|14|
|5|16|
|10|18|
|20|20|
|50|22|
|100|24|
|200|26|
|500|28|
|1000|30|

The maximum balance discount is 30 at the registered balance of 1000 coins. The maximum combined wallet and balances discount is 60, which is an amount large enough to give any individual a chance to compete is the most advanged pools of mining resources.

## Beer Coin Mining Page

The [BEER Coin](https://polybeertoken.github.io) mining page consists of several sections that provide all information and functionality to start mining the BEER Coins.

## Tutorial for Beginners

Even a total beginners can start earning BEER Coins on Polygon without having any mining experience or having to deposit any of their money.

- Install [Metamask](https://metamask.io) in your browser
- Add minimum funds to your wallet through [Matic Faucet](https://macncheese.finance/matic-polygon-mainnet-faucet.php), so you could make transactions
- Go to [BEER Coin](https://polybeertoken.github.io) mining page to start mining
