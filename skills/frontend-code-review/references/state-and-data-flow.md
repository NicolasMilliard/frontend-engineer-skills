# State and data flow

Use this reference when a change involves local state, effects, async work, URL state, shared client state, server data, or cache behavior. Trace who owns each value and what users observe as it moves between states; do not review by hook preference.

## Derived state and effects

**Raise when:** A component stores a value that can be derived from current props or state and synchronization can observably lag, overwrite user input, or diverge on an update path. Also raise an effect whose external resource can outlive the component or change of inputs without the corresponding cleanup or resubscription.

**Do not raise when:** State intentionally preserves history, a draft, an optimistic value, or a user choice that is no longer a pure function of current inputs. Do not object to `useEffect` merely because computation could happen elsewhere: effects are appropriate for synchronizing with subscriptions, timers, browser APIs, network work, and other systems outside React.

## Async transitions

**Raise when:** A later render can leave older work able to win—for example, a response for a previous search key can overwrite the current result—or when rejection, cancellation, or a non-success HTTP response leaves the UI in a false success or permanent loading state. Describe the concrete ordering that causes the failure; cancellation, request identity, or guarded commits are possible remedies rather than universal requirements.

**Do not raise when:** The operation cannot overlap, stale completion is harmless by contract, or the existing data layer already enforces the needed ordering and cleanup. Fetching in an effect is not itself a defect, and a query library is not required when the local lifecycle is correct and proportionate.

## State ownership and boundaries

**Raise when:** The same authority is duplicated across local, URL, shared-client, and server state with no clear reconciliation rule; a refresh, navigation, second consumer, or server revalidation can therefore produce conflicting behavior. At a server-to-client boundary, raise data that is unnecessarily exposed, is not serializable, or transfers broader records than the client contract needs.

**Do not raise when:** Copies have distinct roles, such as a server snapshot feeding an explicitly local editable draft, or when colocated state has one owner and no other consumer needs to control or persist it. Do not push state upward, into the URL, or into a global store without a concrete ownership or synchronization problem.

## Caching and identity

**Raise when:** Cache keys omit behavior-changing inputs, invalidation cannot follow a mutation, or identity churn demonstrably causes stale data, duplicate requests, or a material render cost. Tie the comment to the user-visible stale path or a plausible cost model.

**Do not raise when:** Recalculation is cheap, object identity has no meaningful downstream effect, or the framework/data layer already supplies the required cache semantics. Do not prescribe memoization or a caching library as a default optimization.

## User-visible states

**Raise when:** Loading, error, empty, stale, or partial-data states are collapsed in a way that lies to the user, hides usable data, strands an essential interaction, or makes retry impossible. Pay particular attention to transitions, such as a refetch after success or an error after partial data, rather than checking only initial render branches.

**Do not raise when:** A distinct state is impossible under the actual contract, is too brief to be observable, or the shared boundary already renders it correctly. Do not demand separate UI for every theoretical state without a reachable behavior to protect.
