# Security Remarks

1\. The only accepted callback function is _fulfillRandomness_(bytes32 \_reqId, uint256 \_random) with the function selector being _0x1f1f897f_. This is required in order to avoid malicious callbacks.&#x20;

2\. The _fulfillRandomness_ function is able to verify the VRF value and minimize the amount of keep3rs that can fulfill data with the following check functions:

* `require(isFulfiller[msg.sender], "Must is fulfiller")` : Hàm fulfillRandomness được đảm chỉ 1 số keep3r cho phép có thể fulfil  data ;
* `require(isNode(validator), "Forbidden validator")`: Số random được đảm bảo sinh ra từ validator oraichain.

3\. The gas limit is 800,000. This means that any fulfilling or callback request exceeding the limit amount can lead to tx getting reverted, at which case the oracle will ignore and refuse to execute such request.

