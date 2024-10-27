# Enhanced POA Token Contract with Multiple Events Support

## Overview
This is a Clarity Smart Contract designed for managing multiple events with rewards, attendance tracking, and verifier management. It provides several functions for event creation, attendance recording, verification, and reward distribution. Additionally, it features administrative functions for managing verifiers, events, and treasury funds.

## Table of Contents
- [Features](#features)
- [Installation & Setup](#installation--setup)
- [Error Codes](#error-codes)
- [Data Structures](#data-structures)
- [Public Functions](#public-functions)
- [Read-Only Functions](#read-only-functions)
- [Helper Functions](#helper-functions)
- [Usage](#usage)
- [Security Considerations](#security-considerations)

## Features
- **Event Management**: Create, activate, and deactivate events with configurable rewards and duration.
- **Attendance Tracking**: Record check-in and check-out times for event attendees.
- **Reward Distribution**: Calculate and distribute base and bonus rewards based on attendance duration.
- **Verifiers Management**: Assign verifiers to validate attendance.
- **Treasury Management**: Handle STX deposits and withdrawals to fund rewards.

## Installation & Setup
1. Deploy the smart contract using the Clarity smart contract deployment tools on a Stacks blockchain testnet or mainnet.
2. Set the contract owner as the principal deploying the contract.
3. Ensure the contract treasury is funded by calling the `deposit-funds` function.

## Error Codes
The contract defines several error codes to handle edge cases and ensure robust operations:
- `ERR-NOT-AUTHORIZED (100)`: Caller lacks permission to perform the action.
- `ERR-ALREADY-CLAIMED (101)`: Reward already claimed by attendee.
- `ERR-EVENT-NOT-ENDED (102)`: Attempt to claim reward before event end.
- `ERR-EVENT-ENDED (103)`: Event has already ended or is inactive.
- `ERR-NO-REWARD (104)`: No valid reward available.
- `ERR-EVENT-NOT-FOUND (105)`: Event ID does not exist.
- `ERR-INSUFFICIENT-FUNDS (106)`: Insufficient funds in the treasury.
- `ERR-INVALID-DURATION (107)`: Invalid event duration specified.
- `ERR-ALREADY-REGISTERED (108)`: User already registered for the event.
- `ERR-INVALID-START-HEIGHT (110)`: Event start height is invalid.
- `ERR-INVALID-REWARD (111)`: Reward value exceeds allowed limits.
- `ERR-INVALID-MIN-ATTENDANCE (112)`: Minimum attendance exceeds event duration.
- `ERR-ALREADY-VERIFIED (122)`: Attendance already verified.
- `ERR-EVENT-NOT-ACTIVE (120)`: The event is no longer active.
- `ERR-INVALID-AMOUNT (1005)`: Invalid amount specified for deposits/withdrawals.

## Data Structures
1. **Events Map (`events`)**: Stores event information such as the name, start and end heights, rewards, and organizer details.
2. **Event Attendance Map (`event-attendance`)**: Tracks attendee check-in and check-out times, attendance duration, and verification status.
3. **Rewards Claimed Map (`rewards-claimed`)**: Stores reward details, including the amount and tier of rewards claimed by attendees.
4. **Verification Details Map (`verification-details`)**: Stores information on verifiers who verified specific attendees.
5. **Verifiers Map (`verifiers`)**: Stores the list of authorized verifiers with a boolean flag indicating their status.

## Public Functions
1. **Create Event (`create-event`)**: Creates a new event with the provided parameters.
    - Parameters:
      - `name`: ASCII string (max 50 chars)
      - `description`: ASCII string (max 200 chars)
      - `start-height`: Event starting block height
      - `duration`: Event duration in blocks
      - `base-reward`: Base reward amount
      - `bonus-reward`: Bonus reward for meeting minimum attendance
      - `min-attendance`: Minimum attendance required to earn a bonus
2. **Check In (`check-in`)**: Registers an attendee's check-in for a specific event.
    - Parameters:
      - `event-id`: Event ID
3. **Check Out (`check-out`)**: Records the attendee’s check-out and calculates attendance duration.
    - Parameters:
      - `event-id`: Event ID
4. **Verify Attendance (`verify-attendance`)**: Verifies an attendee's participation in an event.
    - Parameters:
      - `event-id`: Event ID
      - `attendee`: Principal address of the attendee
5. **Claim Reward (`claim-reward`)**: Allows attendees to claim their reward after event completion.
    - Parameters:
      - `event-id`: Event ID
6. **Add Verifier (`add-verifier`)**: Adds a new verifier.
    - Parameters:
      - `address`: Principal address of the verifier
7. **Remove Verifier (`remove-verifier`)**: Removes an existing verifier.
    - Parameters:
      - `address`: Principal address of the verifier
8. **Deactivate Event (`deactivate-event`)**: Deactivates an event to prevent further participation.
    - Parameters:
      - `event-id`: Event ID
9. **Deposit Funds (`deposit-funds`)**: Deposits STX tokens into the contract's treasury.
    - Parameters:
      - `amount`: Amount of STX to deposit
10. **Withdraw Funds (`withdraw-funds`)**: Withdraws STX from the treasury by the contract owner.
     - Parameters:
        - `amount`: Amount of STX to withdraw

## Read-Only Functions
1. **Get Owner (`get-owner`)**: Returns the contract owner's principal address.
2. **Get Event (`get-event`)**: Fetches the details of a specific event by ID.
    - Parameters:
      - `event-id`: Event ID
3. **Get Attendance Record (`get-attendance-record`)**: Retrieves the attendance record for an attendee at a specific event.
    - Parameters:
      - `event-id`: Event ID
      - `attendee`: Principal address of the attendee
4. **Get Reward Claim (`get-reward-claim`)**: Returns the reward claim details for an attendee.
    - Parameters:
      - `event-id`: Event ID
      - `attendee`: Principal address of the attendee

## Helper Functions
1. **is-valid-ascii (Private)**: Validates if a string contains only valid ASCII characters.
2. **can-verify-attendance (Read-only)**: Checks if an attendee's participation can be verified.
    - Parameters:
      - `event-id`: Event ID
      - `attendee`: Principal address of the attendee

## Usage
- **Creating Events**: Use the `create-event` function to create events with rewards and attendance requirements.
- **Registering Attendance**: Attendees check in and check out via the `check-in` and `check-out` functions.
- **Verification**: Verifiers validate attendance using the `verify-attendance` function.
- **Claiming Rewards**: Attendees claim rewards after verification using `claim-reward`.
- **Managing Treasury**: Admins can deposit and withdraw STX funds using `deposit-funds` and `withdraw-funds`.

## Security Considerations
- Ensure only authorized verifiers are added to the contract.
- Regularly audit the contract for potential vulnerabilities.
- Maintain sufficient funds in the treasury to cover rewards.
