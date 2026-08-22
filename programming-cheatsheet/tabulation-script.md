# HTML Image Gallery Generator
* This script generates an HTML table displaying images from multiple case directories.
* Each row represents a case, and each column represents a different image suffix.

```python
import os

# Change these to your suffixes (column order)
SUFFIXES = [
    "yourSuffix.png",
    "yourSuffix2.png",
]

def find_image(case_dir, prefix, suffix):
    filename = f"{prefix}{suffix}"
    filepath = os.path.join(case_dir, filename)
    # Use only if it actually exists
    if os.path.isfile(filepath):
        # Return relative path from within results dir (i.e., './dir1/image_suffix.png')
        return f"./{os.path.basename(case_dir)}/{filename}"
    else:
        return None

def generate_html(result_dir, case_dirs):
    html = []
    html.append("<!DOCTYPE html>")
    html.append("<html>\n<head>")
    html.append("<style>")
    html.append("th { position: sticky; top: 0px; background: white; }")
    html.append(
        "table, td, th { border: 1px solid #aaa; border-collapse: collapse; padding: 8px; }"
    )
    html.append("</style>\n</head>\n<body>")
    html.append("<table>")

    # Header
    html.append(
        "  <tr><th>Name</th>"
        + "".join(f"<th>{suffix.replace('.png', '')}</th>" for suffix in SUFFIXES)
        + "</tr>"
    )

    # For each case dir build row
    for case_name in sorted(case_dirs):
        case_dir = os.path.join(result_dir, case_name)

        # Infer prefix by scanning for first file with '_yourSuffix.png', '_yourSuffix2.png', etc.
        # Usually prefix is the base name before first _
        found_prefix = None
        files = os.listdir(case_dir)
        prefix_for_suffix = {}
        for suffix in SUFFIXES:
            for fn in files:
                if fn.endswith(suffix):
                    possible_prefix = fn[:-len('_' + suffix)]
                    prefix_for_suffix[suffix] = possible_prefix
                    break

        if not prefix_for_suffix:
            continue  # skip cases with no matching files

        row = [f"<td>{case_name}</td>"]
        for suffix in SUFFIXES:
            prefix = prefix_for_suffix.get(suffix, None)
            img_path = find_image(case_dir, prefix, suffix) if prefix else None
            if img_path:
                cell = (
                    f'<a href="{img_path}">' f'<img src="{img_path}" width="420"></a>'
                )
            else:
                cell = ""
            row.append(f"<td>{cell}</td>")

        html.append("  <tr>" + "".join(row) + "</tr>")

    html.append("</table>\n</body>\n</html>")
    return "\n".join(html)


if __name__ == "__main__":
    # Let user set directory if desired
    result_dir = "results"  # change as needed

    # Find all case directories (e.g. case1, case2, ...)
    case_dirs = [
        d for d in os.listdir(result_dir) if os.path.isdir(os.path.join(result_dir, d))
    ]

    html_code = generate_html(result_dir, case_dirs)
    save_path = os.path.join(result_dir, "results.html")

    with open(save_path, "w", encoding="utf-8") as f:
        f.write(html_code)

    print(f"HTML saved at {save_path}")

```