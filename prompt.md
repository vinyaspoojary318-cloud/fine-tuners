Dispatches may be cancelled at any point before they are completed. Check each state change against the allowed transitions. If a transition is not valid, reject it without changing the dispatch or related state.

The dispatch lifecycle is:

- `requested`: no seats are reserved yet.
- `matched`: seats are held for the students.
- `guardian_consented`: required guardian consent has been confirmed.
- `approved`: the coordinator has authorised the trip.
- `dispatched`: the vehicle is on its way.
- `completed`: the trip is finished and its impact can be recorded.

Check vehicle capacity again immediately before coordinator approval. Give each dispatch a unique ID, and make approval idempotent so approving the same dispatch twice has no additional effect.

When a dispatch is cancelled before completion:

- Release its reserved seats.
- Restore any route occupancy changed for the dispatch.
- Reopen eligible student requests.
- Remove any provisional impact metrics.
- Keep the decision history, and mark the dispatch as cancelled.

Do not use the ordinary cancellation flow to undo a completed trip. Leave a documented stub for a separate administrative correction flow.

Add a `DecisionLogEntry` for every dispatch transition.

Use `useReducer` as the single source of truth. Save state to localStorage in a versioned envelope with this shape:

```ts
{
  version: number;
  savedAt: string;
  state: MobilityState;
}
```
