# Connectivity Verification (Task 4)

## 1. Basic reachability — should all succeed (routing works between VLANs)

| From | To | Expected |
|------|----|-----------|
| Eng-PC1 (10.0.0.1) | Eng-PC2 (10.0.0.2) | ✅ success (same VLAN) |
| Eng-PC1 (10.0.0.1) | HR-PC1 (10.0.0.65) | ✅ success (routed via R1) |
| Eng-PC1 (10.0.0.1) | Sales-PC1 (10.0.0.130) | ✅ success (routed via R1) |
| HR-PC1 (10.0.0.65) | Sales-PC2 (10.0.0.129) | ✅ success (routed via R1) |
| Every PC | its own gateway | ✅ success |

If any of these fail, check: PC IP/mask/gateway, SW1 port VLAN assignment,
R1 interface IP/no-shutdown, and physical cabling in Packet Tracer.

## 2. Broadcast ping test (proves VLAN = separate broadcast domain)

Run each of these from **one PC in that VLAN**, using **Simulation Mode** so you can
watch which devices actually receive the broadcast frame:

| Ping target | From VLAN | Devices that SHOULD receive it |
|-------------|-----------|----------------------------------|
| 10.0.0.63  (Eng broadcast)  | 10 (Engineering) | Only Eng-PC1, Eng-PC2, SW1, R1's G0/0 — **not** HR or Sales PCs |
| 10.0.0.127 (HR broadcast)   | 20 (HR)          | Only HR-PC1, HR-PC2, SW1, R1's G0/1 — **not** Eng or Sales PCs |
| 10.0.0.191 (Sales broadcast)| 30 (Sales)       | Only Sales-PC1, Sales-PC2, SW1, R1's Gig0/2 — **not** Eng or HR PCs |

### How to observe in Packet Tracer
1. Switch to **Simulation Mode** (bottom-right).
2. Open the source PC's Command Prompt and run `ping 10.0.0.63` (example for VLAN10).
3. Click **Auto Capture / Play** and watch the animated envelope icons.
4. Confirm the broadcast (flood) only travels to devices in the *same* VLAN's access
   ports — PCs in other VLANs, and the other R1 interfaces, should show no traffic.
   This confirms each VLAN is an isolated broadcast domain and that only routing (not
   flooding) carries traffic between them.
