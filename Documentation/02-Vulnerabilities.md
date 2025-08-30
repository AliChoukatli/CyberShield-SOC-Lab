
# To make our 2 machines vulnerable 

- we need to remove the rule for RDP port 3389

![NSG_Windows_Delete_RDP_Rule](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Windows_Delete_RDP_Rule.png)

- then we create a new rule :

![NSG_Win_Rule_any](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Win_Rule_any.png)

- we will delete ssh rule

![NSG_Windows_Delete_SSH_Rule](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_Windows_Delete_SSH_Rule.png)

- create rule any linux 

![NSG_Linux_Rule_any](https://github.com/AliChoukatli/Azure-Honeynet-SOC-Lab/blob/main/Screenshots/NSG_linux_Rule_any.png)

---
