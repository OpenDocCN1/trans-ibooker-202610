1. Create a new C# script called DamageReceiver.cs that can receive a signal that an object has taken damage, and has a number of hit points that can be diminished:

```
```csharp
public class DamageReceiver : MonoBehaviour {
    UnityEngine.Events.UnityEvent onDeath;

    [SerializeField] int hitPoints = 5;
    int currentHitPoints;

    private void Awake() {
        currentHitPoints = hitPoints;
    }

    public void TakeDamage(int damageAmount) {
        currentHitPoints -= damageAmount;

        if (currentHitPoints <= 0) {
            if (onDeath != null) {
                onDeath.Invoke();
            }
            Destroy(gameObject);
        }
    }
}
```
```