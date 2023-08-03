# Kiermet
 struct feeProportions {
    uint liquidity;
    uint tokenFee;
    uint TreasuryFee;
    uint RevenueFee;
    }

    feeProportions public Ratios = feeProportions(
    { liquidity: 0, tokenFee: 0, TreasuryFee: 1000, RevenueFee: 0});

    uint256 private constant masterTaxDivisor = 10000;
    uint256 private constant MAX = ~uint256(0);
    uint8 constant private _decimals = 9;
 
    uint256 private _tTotal = startingSupply * 10**_decimals;
    uint256 private _tFeeTotal;

    IUniswapV2Router02 public dexRouter;
    address public lpPair;

    address constant private _routerAddress = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D; // UNISWAP ROUTER
    
    address constant public DEAD = 0x000000000000000000000000000000000000dEaD;
    
    address payable public _treasuryWallet = payable(0xbE47c9aa991189EeadD5279A90f447729aEC326A);  // Treasury, receives taxes
    address payable public _revenueWallet = payable(0x2858f2C1D89e547c66302ffBCF311109Ec8347C9);  // This wallet will deploy the revenue share dApp.
    
    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = false;
    
    uint256 private maxTxPercent = 2;
    uint256 private maxTxDivisor = 100;
    uint256 private _maxTxAmount = (_tTotal * maxTxPercent) / maxTxDivisor;
    
    uint256 private maxWalletPercent = 2;
    uint256 private maxWalletDivisor = 100;
    uint256 private _maxWalletSize = (_tTotal * maxWalletPercent) / maxWalletDivisor;
    
    uint256 private swapThreshold = (_tTotal * 5) / 10_000;
    uint256 private swapAmount = (_tTotal * 5) / 1_000;

    bool private sniperProtection = true;
    bool public _hasLiqBeenAdded = false;
    uint256 private _liqAddStatus = 0;
    uint256 private _liqAddBlock = 0;
    uint256 private _liqAddStamp = 0;
    uint256 private _initialLiquidityAmount = 0; // make constant
    uint256 private snipeBlockAmt = 0;
    uint256 public snipersCaught = 0;
