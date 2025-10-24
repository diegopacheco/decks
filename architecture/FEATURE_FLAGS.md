# Feature Flags

Feature flags are runtime configuration switches that enable or disable functionality without deploying new code. They decouple deployment from release, allowing gradual rollouts and instant rollbacks, migrations and A/B testing.

## Strategies

**Environment Variables**: Simplest approach. Requires restart to change. Good for operational flags.

**Configuration Files**: Load from files on startup or reload periodically. No restart needed if hot-reloaded.

**Database or Cache**: Dynamic flags stored in database or distributed cache. Change instantly across all instances.

**Feature Flag Services**: Dedicated systems like LaunchDarkly, Split, or Unleash. Advanced targeting and analytics.

## Targeting and Segmentation

Flags can target based on:
* User ID or email for specific users
* Percentage rollout for gradual releases
* Geographic region or data center
* User attributes like subscription tier
* Environment like staging vs production
* Random sampling for experiments

## Anti-Patterns

**Long-Lived Release Flags**: Release flags should be temporary. Remove after release complete.

**Nested Flags**: Flags inside flags create exponential complexity. Avoid deeply nested conditions.

**Flag Proliferation**: Too many flags make system hard to reason about. Be selective.

**Using Flags for Configuration**: Feature flags are not general configuration. Use proper config systems for that.