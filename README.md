<img src="assets/logo.png" width="88" height="88" alt="Blyx Moments logo">

# Blyx Moments

Summarize paired observations as they arrive and merge statistics from independent batches. Use one six-number summary for each disjoint batch. Merge them when the raw observations are no longer available. Empty batches have an identity summary, which makes it practical to combine results from workers.

## Use it in a project

Install [Blyx 0.2.1](https://github.com/Rahulchaube1/blyxxxx/releases/tag/v0.2.1), Rust 1.85 or newer, and a host linker for native compilation. From the directory containing your entry file, pin the library with Git.

```sh
git clone --depth 1 --branch v0.1.0 https://github.com/Rahulchaube1/blyx-moments.git vendor/moments
```

Save this program as `main.blyx` beside `vendor` and run `blyxc native-run main.blyx`.

```blyx
use vendor::moments::moments;

fn main() {
    let morning = mom_summary([1, 2], [10, 12]);
    let afternoon = mom_summary([3], [14]);
    let day = mom_merge(morning, afternoon);
    println!(at(day, 0));
    println!(at(day, 2));
    println!(mom_sample_sd_y(day));
}
```

Expected output:

```text
3
12
2
```

[Run this example in your browser](https://www.blyx-lang.space/play?example=package-moments)

## API and data contract

| Function | Result |
| --- | --- |
| `mom_empty()` | An empty paired summary. |
| `mom_update(state, x, y)` | A new summary including one finite observation. |
| `mom_summary(xs, ys)` | Summarize two equally sized numeric vectors. |
| `mom_merge(a, b)` | Combine summaries for disjoint sets of observations. |
| `mom_sample_sd_y(state)` | Sample standard deviation of y, with denominator n−1. It returns −1 for an invalid state or fewer than two rows. |

The state layout is `[n, mean_x, mean_y, Sxx, Sxy, Syy]`. The last three fields are centered sums, so sample covariance is Sxy/(n−1) for n ≥ 2. Functions use online mean updates and pairwise moment combination. Each row takes constant state and work. Merging twice counts the data twice. Summation order can change the last bits. Vector operations return an empty vector for invalid inputs or unrepresentable results. Use `mom_valid` to check the representation of a stored state.

## Contribute

Clone this repository, run `blyxc native-run tests.blyx` and `blyxc native-run example.blyx`, and include a failing example with any behavior change. Include the compiler version, inputs and expected output in bug reports. [Open a package issue](https://github.com/Rahulchaube1/blyx-moments/issues) · [Blyx handbook](https://doc.blyx-lang.space/guide/training-and-monitoring) · [Ecosystem catalog](https://www.blyx-lang.space/packages)

## Connect with Rahul Chaube

[Portfolio](https://www.rahulchaube1.com/) · [GitHub](https://github.com/Rahulchaube1) · [LinkedIn](https://www.linkedin.com/in/rahulchaube1) · [Hugging Face](https://huggingface.co/oncody) · [X](https://x.com/RahulChaube_) · [EverestQ](https://www.everestqai.com/) · [Contact](https://www.rahulchaube1.com/contact)

[Blyx website](https://www.blyx-lang.space/) · [BlyxTorch](https://www.blyx-lang.space/packages/blyxtorch) · [Handbook](https://doc.blyx-lang.space/) · [Community](https://www.blyx-lang.space/community)

Support Rahul’s open-source work through [GitHub Sponsors](https://github.com/sponsors/Rahulchaube1) or [Buy Me a Coffee](https://buymeacoffee.com/rahulchaube).
