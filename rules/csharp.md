# C# (Unity)

### CS-001: No allocations in per-frame code
- **Priority:** Performance
- **Applies to:** `Assets/**/*.cs`
- **Rule:** `Update`, `LateUpdate`, `FixedUpdate`, `OnGUI` and code they call every frame allocate nothing: no LINQ, no `new` of classes or arrays, no string concatenation / interpolation, no closures capturing locals, no `GetComponent` in loops (cache it).
- **Why:** GC spikes are frame-time spikes.

### CS-002: Serialized fields, not public fields
- **Priority:** Best Practices
- **Applies to:** `Assets/**/*.cs`
- **Rule:** inspector data is `[SerializeField] private`; public API goes through properties or methods. Renaming a serialized field keeps data with `[FormerlySerializedAs]`.
- **Why:** encapsulation, and a rename must not silently reset scene / prefab values.

### CS-003: Unsubscribe what you subscribe
- **Priority:** Critical
- **Applies to:** `Assets/**/*.cs`
- **Rule:** every `+=` on an event / delegate in `OnEnable` / `Awake` / `Start` has a matching `-=` in `OnDisable` / `OnDestroy`; static events are always unsubscribed.
- **Why:** leaks and calls into destroyed objects (MissingReferenceException).

### CS-004: No Find / FindObjectOfType at runtime
- **Priority:** Architecture
- **Applies to:** `Assets/**/*.cs`
- **Rule:** no `GameObject.Find*`, `FindObjectOfType`, `FindObjectsOfType` in gameplay code paths; use serialized references, injection or a registry. Editor tools and one-time bootstrap are fine.
- **Why:** slow, fragile to renames and hierarchy changes.

### CS-005: Null checks on UnityEngine.Object
- **Priority:** Critical
- **Applies to:** `Assets/**/*.cs`
- **Rule:** never use `?.`, `??` or `is null` on `UnityEngine.Object` types; use `== null` / implicit bool.
- **Why:** Unity overrides `==` for destroyed objects; the C# operators bypass it.

### CS-006: Coroutines and async stop with their owner
- **Priority:** Critical
- **Applies to:** `Assets/**/*.cs`
- **Rule:** long-running coroutines / async tasks stop when the object is disabled or destroyed (`StopCoroutine`, `CancellationToken` from `destroyCancellationToken` or an owned source).
- **Why:** work continuing on a destroyed object.

### CS-007: Naming
- **Priority:** Style
- **Applies to:** `Assets/**/*.cs`
- **Rule:** PascalCase for types, methods, properties, events; `_camelCase` for private fields; camelCase for locals and parameters; one public type per file, file named after it.

# Process

### PR-001: Small, focused changes
- **Priority:** Best Practices
- **Applies to:** PR process
- **Rule:** one task per branch; no unrelated scene / prefab / ProjectSettings churn; `.meta` files travel with their assets.
