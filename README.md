// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MintableToken is ERC20, Ownable {
    constructor() ERC20("MintableToken", "MTK") {}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract BurnableToken is ERC20Burnable {
    constructor() ERC20("BurnableToken", "BTK") {
        _mint(msg.sender, 100000 * (10 ** decimals()));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Pausable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract PausableToken is ERC20Pausable, Ownable {
    constructor() ERC20("PausableToken", "PTK") {
        _mint(msg.sender, 100000 * (10 ** decimals()));
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol";
import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

contract UpgradeableToken is Initializable, ERC20Upgradeable {
    function initialize(uint256 initialSupply) public initializer {
        __ERC20_init("UpgradeableToken", "UTK");
        _mint(msg.sender, initialSupply * (10 ** decimals()));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";
import "@openzeppelin/contracts/governance/Governor.sol";

contract GovernanceToken is ERC20Votes {
    constructor(uint256 initialSupply) ERC20("GovernanceToken", "GTK") ERC20Permit("GovernanceToken") {
        _mint(msg.sender, initialSupply * (10 ** decimals()));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Snapshot.sol";

contract DividendToken is ERC20, ERC20Snapshot {
    constructor() ERC20("DividendToken", "DTK") {
        _mint(msg.sender, 100000 * (10 ** decimals()));
    }

    function snapshot() public {
        _snapshot();
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract StakingToken is ERC20 {
    mapping(address => uint256) public stakedBalances;

    constructor() ERC20("StakingToken", "STK") {
        _mint(msg.sender, 100000 * (10 ** decimals()));
    }

    function stake(uint256 amount) public {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");
        _transfer(msg.sender, address(this), amount);
        stakedBalances[msg.sender] += amount;
    }

    function unstake(uint256 amount) public {
        require(stakedBalances[msg.sender] >= amount, "Insufficient staked balance");
        stakedBalances[msg.sender] -= amount;
        _transfer(address(this), msg.sender, amount);
    }
}
