# Linux Distribution Selection Justification

## Evaluation Criteria
The server distribution was evaluated against the following criteria:
- Long-term security updates
- Stability and predictability
- Package management ecosystem
- Documentation and community support
- Suitability for headless server deployment

## Distribution Comparison

| Distribution | Pros | Cons |
|--------------|------|------|
| Ubuntu Server LTS | Long-term support (5 years), excellent documentation, wide package availability, strong security update process | Slightly slower package versions |
| Debian Stable | Extremely stable, minimal default install, strong security model | Older packages, less beginner-friendly |
| CentOS Stream | Enterprise-aligned tooling | Rolling-release model introduces instability |
| Arch Linux | Very current packages, full control | High maintenance, unsuitable for production servers |

## Selected Distribution: Ubuntu Server LTS

Ubuntu Server LTS was selected due to its balance between stability, security, and usability. The predictable release cycle and extensive documentation reduce administrative risk while supporting industry-standard practices such as unattended security updates and hardened SSH configurations.

## Trade-off Analysis
Choosing Ubuntu Server prioritises operational stability and security support over access to bleeding-edge packages. This trade-off is appropriate for a production-style server environment where predictability and security outweigh novelty.
