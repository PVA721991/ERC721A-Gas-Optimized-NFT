# ERC721A-Gas-Optimized-NFT
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract GasOptimizedNFT {
    string public name = "Base Optimized NFT";
    string public symbol = "BONFT";

    mapping(uint256 => address) public ownerOf;
    mapping(address => uint256) public balanceOf;

    uint256 public totalSupply;
    uint256 public maxSupply = 5000;

    address public owner;

    error NotOwner();
    error MaxSupplyReached();

    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    constructor() {
        owner = msg.sender;
    }

    function mint(uint256 quantity) public {
        if (msg.sender != owner) revert NotOwner();
        if (totalSupply + quantity > maxSupply) revert MaxSupplyReached();

        for (uint256 i = 0; i < quantity; i++) {
            uint256 tokenId = totalSupply + i + 1;
            ownerOf[tokenId] = msg.sender;
            emit Transfer(address(0), msg.sender, tokenId);
        }
        balanceOf[msg.sender] += quantity;
        totalSupply += quantity;
    }
}
