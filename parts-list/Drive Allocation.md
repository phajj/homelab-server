## Storage Layout and Usage Plan

### SSD Allocation Overview

| Drive                   | Capacity | Interface | Role                                   | TrueNAS Use                                  | Unraid Use                                        | Notes                                                                 |
|------------------------|----------|-----------|----------------------------------------|----------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------|
| Kingston SNV2S2000G | 2TB      | Gen 4     | Proxmox VM storage                     | Dataset or ZFS pool for VMs                  | Cache pool for VMs and Docker                   | Fastest NVMe drive for active workloads                              |
| Kingston NV2             | 1TB      | Gen 4     | ZFS SLOG (write cache)                 | Dedicated SLOG device for sync writes        | Cache drive (fast writes) or Docker metadata    | Must be fast + reliable; ideally PLP if used for SLOG                |
| TEAMGROUP MP33           | 2TB      | Gen 3     | ZFS L2ARC (read cache)                 | L2ARC read cache for metadata and hot reads  | Secondary cache or appdata drive                | Can be removed later if needed to free NVMe slot                     |
| TEAMGROUP T-Force Vulcan Z  | 512GB    | SATA III  | Proxmox boot drive                     | N/A                                          | N/A                                              |                    |

---

### HDD Storage Pool (Primary Data Pool)

| Drives Used           | RAID Level | Capacity (usable) | TrueNAS Option             | Unraid Option                     | Notes                                                                 |
|-----------------------|------------|--------------------|-----------------------------|-----------------------------------|-----------------------------------------------------------------------|
| 4 x HDDs (Identical)  | RAID 10    | 2x drive capacity  | ZFS Mirror vdevs (RAID10)  | Main array with dual parity | Balanced speed + redundancy; great for mixed read/write workloads     |
