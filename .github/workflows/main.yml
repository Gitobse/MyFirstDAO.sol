// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract DAO {

		// Struct to represent a proposal in the DAO	
		struct Proposal {
		    string description;
		    uint voteCount;
		    bool executed;
		}
		
		// Struct to represent a member of the DAO
		struct Member {
		    address memberAddress;
		    uint memberSince;
		    uint tokenBalance;
		} 
		
		address[] public members; // Array to store member addresses
		mapping(address => Member) public memberInfo; // Mapping to store member information
		mapping(address => mapping(uint => bool)) public votes; // Mapping to track member votes for proposals
		Proposal[] public proposals; // Array to store proposals
		
		uint public totalSupply; // Total token supply
		mapping(address => uint) public balances; // Mapping to store token balances of members
		
		event ProposalCreated(uint indexed proposalId, string description); // Event emitted when a proposal is created
		event VoteCast(address indexed voter, uint indexed proposalId, uint tokenAmount); // Event emitted when a vote is cast
		event ProposalAccepted(string message); // Event emitted when a proposal is accepted
		event ProposalRejected(string rejected); // Event emitted when a proposal is rejected
		
		// Function to add a member to the DAO
		function addMember(address _member) public {
		    require(memberInfo[_member].memberAddress == address(0), "Member already exists");
		    memberInfo[_member] = Member({
		        memberAddress: _member,
		        memberSince: block.timestamp,
		        tokenBalance: 100
		    });
		    members.push(_member);
		    balances[_member] = 100;
		    totalSupply += 100;
		} 
		
		// Function to remove a member from the DAO
		function removeMember(address _member) public {
		    require(memberInfo[_member].memberAddress != address(0), "Member does not exist");
		    memberInfo[_member] = Member({
		        memberAddress: address(0),
		        memberSince: 0,
		        tokenBalance: 0
		    });
		    for (uint i = 0; i < members.length; i++) {
		        if (members[i] == _member) {
		            members[i] = members[members.length - 1];
		            members.pop();
		            break;
		        }
		    }
		    balances[_member] = 0;
		    totalSupply -= 100;
		} 
		
		// Function to create a proposal in the DAO
		function createProposal(string memory _description) public {
		    proposals.push(Proposal({
		        description: _description,
		        voteCount: 0,
		        executed: false
		    }));
		    emit ProposalCreated(proposals.length - 1, _description);
		} 
		
		// Function for a member to vote on a proposal
		function vote(uint _proposalId, uint _tokenAmount) public {
		    require(memberInfo[msg.sender].memberAddress != address(0), "Only members can vote");
		    require(balances[msg.sender] >= _tokenAmount, "Not enough tokens to vote");
		    require(votes[msg.sender][_proposalId] == false, "You have already voted for this proposal");
		    votes[msg.sender][_proposalId] = true;
		    memberInfo[msg.sender].tokenBalance -= _tokenAmount;
		    proposals[_proposalId].voteCount += _tokenAmount;
		    emit VoteCast(msg.sender, _proposalId, _tokenAmount);
		} 
		
		// Function to execute a proposal in the DAO
		function executeProposal(uint _proposalId) public {
		    require(proposals[_proposalId].executed == false, "Proposal has already been executed");
		    if(((proposals[_proposalId].voteCount / totalSupply)*100) > 50){
		        proposals[_proposalId].executed = true;
		        emit ProposalAccepted("Proposal has been approved");
		    }
		    emit ProposalRejected("Proposal has not been approved by majority vote");
		} 
}

