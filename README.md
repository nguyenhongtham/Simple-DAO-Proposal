# Simple-DAO-Proposal
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MiniDAO {
    struct Proposal {
        string description;
        uint256 votesFor;
        uint256 votesAgainst;
        uint256 deadline;
        bool executed;
    }

    mapping(uint256 => Proposal) public proposals;
    mapping(address => uint256) public votingPower;
    uint256 public proposalCount;

    function createProposal(string memory _desc, uint256 _durationDays) public {
        proposals[proposalCount] = Proposal({
            description: _desc,
            votesFor: 0,
            votesAgainst: 0,
            deadline: block.timestamp + _durationDays * 1 days,
            executed: false
        });
        proposalCount++;
    }

    function vote(uint256 _proposalId, bool support) public {
        require(block.timestamp < proposals[_proposalId].deadline, "Voting ended");
        if (support) {
            proposals[_proposalId].votesFor += votingPower[msg.sender];
        } else {
            proposals[_proposalId].votesAgainst += votingPower[msg.sender];
        }
    }
}
