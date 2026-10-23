```
//          Poisson-disc-sampling-Unity.html
// http://www.cs.ubc.ca/~rbridson/docs/
//          bridson-siggraph07-poissondisk.pdf

// Generates a distribution of 2D points that aren't too close to each
// other. Operates in O(N) time.
public class PoissonDiscSampler
{
    // Maximum number of attempts before marking a sample as inactive.
    private const int k = 30;

    // The rectangle in which points will be placed
    private readonly Rect rect;

    // radius squared
    private readonly float radius2;

    // The cell size of the grid of points
    private readonly float cellSize;

    // The grid of points
    private Vector2[,] grid;

    // The list of locations near which we're trying to add new
    // points.
    private List<Vector2> activeSamples = new List<Vector2>();

    // Create a sampler with the following parameters:
    //
    // - width: each sample's x coordinate will be between [0, width]
    // - height: each sample's y coordinate will be between [0, height]
    // - radius: each sample will be at least `radius` units away from
    //   any other sample, and at most 2 * `radius`.
    public PoissonDiscSampler(float width, float height, float radius)
    {
        rect = new Rect(0, 0, width, height);
        radius2 = radius * radius;
        cellSize = radius / Mathf.Sqrt(2);
        grid = new Vector2[Mathf.CeilToInt(width / cellSize),
                          Mathf.CeilToInt(height / cellSize)];
    }

    // Return a lazy sequence of samples. You typically want to call
    // this in a foreach loop, like so:
    //     foreach (Vector2 sample in sampler.Samples()) { ... }
    public IEnumerable<Vector2> Samples()
    {
        // First sample is chosen randomly
        Vector2 firstSample = new Vector2(Random.value * rect.width,
                                          Random.value * rect.height);
```