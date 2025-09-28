# EKINCH 

cardano atomic swap bidirectional 
Escrow contract on the EVM side should be deployed using Limit Order Protocol's "fillOrderArgs" 
done in the evm side of the contracts 

The maker initiates the swap by creating a detailed order containing:

interface SwapOrder {
  fromToken: string;      // Token address on source chain
  toToken: string;        // Token identifier on destination chain  
  fromAmount: bigint;     // Amount to swap
  toAmount: bigint;       // Expected return amount
  hashlock: string;       // SHA-256 hash of secret
  makerAddress: string;   // Maker's address
  deadline: number;       // Expiration timestamp
  salt: string;          // Unique identifier
}


Limit Order Protocol (LOP) Pre-Interaction
The maker calls the LOP contract's pre-interaction function:

For ETH swaps:

function preInteraction(
    bytes32 orderHash,
    address asset,        // address(0) for ETH
    uint256 amount
) external payable {
    // Validates order signature
    // Locks ETH in LOP contract
    // Marks order as pre-validated
}

 Cross-Chain Escrow Creation
4. Source Escrow Deployment
The EscrowFactory creates a minimal proxy (EIP-1167) pointing to the EscrowSrc implementation:

contract EscrowSrc {
    struct EscrowData {
        bytes32 hashlock;
        address maker;
        address resolver;
        uint256 deadline;
        address asset;
        uint256 amount;
    }
    
    function withdraw(bytes32 secret) external {
        require(sha256(secret) == hashlock, "Invalid secret");
        require(block.timestamp <= deadline, "Deadline passed");
        // Transfer tokens to resolver
    }
}


