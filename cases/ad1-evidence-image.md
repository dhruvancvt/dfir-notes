# AD1 forensic image (Evidence.ad1, ~282 MB)

- **Type:** Logical evidence image analysis
- **Goal:** "When was X executed?"

## Format notes
- AccessData **Logical Image**: signature `ADSEGMENTEDFILE`, then `ADLOGICALIMAGE` at offset 512.
- **FTK Imager** (Windows) is the standard reader. **Autopsy / libewf do not read AD1.**
- The tree is built from item entries with **child / sibling pointers**. It can be
  walked directly with a small parser when FTK Imager isn't available.

## Execution artifacts pulled
| Artifact | Location | Gives you |
|----------|----------|-----------|
| Prefetch | `C:\Windows\Prefetch\*.pf` | run count, last 8 run times |
| Amcache | `C:\Windows\AppCompat\Programs\Amcache.hve` | path, SHA1, first-seen |
| ShimCache | `SYSTEM\...\AppCompatCache` | path, last-modified (existence, not proof of exec on Win10+) |
| UserAssist | `NTUSER\...\Explorer\UserAssist` (ROT13) | GUI launches, count, last run |
| BAM/DAM | `SYSTEM\...\Services\bam\State\UserSettings\<SID>` | last exec time per user |

See [forensic artifacts](../techniques/forensic-artifacts.md).
