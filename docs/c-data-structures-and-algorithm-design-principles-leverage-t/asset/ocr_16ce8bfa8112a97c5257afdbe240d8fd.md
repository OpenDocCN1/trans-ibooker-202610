```
for (auto const i : degrees)
{
    if (assigned_colors[i.first] != 0)
        continue;

    auto outgoing_edges = G.outgoing_edges(i.first);
    std::set<size_t> neighbour_colors;

    // We assume that the graph is bidirectional
    for (auto e : outgoing_edges)
    {
        auto dest_color = assigned_colors[e.dest];
        neighbour_colors.insert(dest_color);
    }

    if (neighbour_colors.find(color_to_be_assigned) == neighbour_colors.end())
        assigned_colors[i.first] = color_to_be_assigned;
}

color_to_be_assigned++;

// If there are no uncolored vertices left, exit
if (std::find(assigned_colors.begin() + 1, assigned_colors.end(),
              0) == assigned_colors.end())
    break;
}

return assigned_colors;
}
```

7. The following function outputs the vector of colors:

```
void print_colors(std::vector<size_t>& colors)
{
    for (auto i = 1; i < colors.size(); i++)
    {
        std::cout << i << ": " << color_map[colors[i]] << std::endl;
    }
}
```