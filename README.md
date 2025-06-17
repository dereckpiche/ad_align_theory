


## AdAlign Derivation (With Inner Expectation)

```math
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}(s_t,a_t,b_t)\,
        \nabla_{\theta_1}
        \mathbb{E}_{a \sim \pi_1(a | s_t)}
        \left[
        Q^{2}(s_t,a,b_t)
        \right]
\Bigg]
```

```math
=\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}(s_t,a_t,b_t)\,
        \sum_{a}
        \nabla_{\theta_1}
        \pi_1(a | s_1)
        Q^{2}(s_t,a,b_t)
\Bigg]
```

```math
=\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}(s_t,a_t,b_t)\,
        \sum_{a}
        \left[
        \pi_1(a | s_t)
        Q^{2}(s_t,a,b_t)
        \nabla_{\theta_1}\log \pi_1(a|s_t)
        +
        \pi_1(a | s_t)
        \nabla_{\theta_1}Q^{2}(s_t,a,b_t)
        \right]
\Bigg]
```

```math
=\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}(s_t,a_t,b_t)\,
        \mathbb{E}_{a \sim \pi_1(a | s_t)} \left[
        Q^{2}(s_t,a,b_t)
        \nabla_{\theta_1}\log \pi_1(a|s_t)
        +
        \nabla_{\theta_1}Q^{2}(s_t,a,b_t) \right]
\Bigg]
```

We have:

```math
\begin{align}
\nabla_{\theta_1}Q^{2}(s_t,a,b_t)
=
\nabla_{\theta_1} \left[r^2(s_t, a, b_t) + \gamma V^2(s') | s_t, a, b_t \right] \\
=
\nabla_{\theta_1} \left[r^2(s_t, a, b_t) + \gamma \mathbb{E}_{s'} \left[ V^2(s') | s_t, a, b_t \right] \right]
=
\gamma \mathbb{E}_{s'} \left[ \nabla_{\theta_1}V^2(s') | s_t, a, b_t \right] \\
=
\mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t+1}^\infty \gamma^{k-t} Q^2_k \left( \nabla_{\theta_1} \log \pi^1(a_k \mid s_k) + \underbrace{ \cancel{ \nabla_{\theta_1} \log \hat{\pi}^2(b_k \mid s_k) } }_{\text{Stop Gradient}}\right) \Bigm| s_t, a, b_t\right] \\
=
\mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t+1}^\infty \gamma^{k-t} Q^2(s_k, a_k, b_k)  \nabla_{\theta_1} \log \pi^1(a_k \mid s_k)  \Bigm| s_t, a, b_t\right]
\end{align}
```

And so:

```math
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}_t\,
        \mathbb{E}_{a \sim \pi_1(a | s_t)} \left[
        Q^{2}(s_t,a,b_t)
        \nabla_{\theta_1}\log \pi_1(a|s_t)
        +
        \nabla_{\theta_1}Q^{2}(s_t,a,b_t) \right]
\Bigg]
```

```math
=
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}_t\,
        \mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t}^\infty \gamma^{k-t} Q^2_k  \nabla_{\theta_1} \log \pi^1(a_k \mid s_k)  \Bigm | s_t, a\sim\pi_1(\cdot|s_t), b_t\right]
\Bigg]
```

### Advantages

Recall that vanishing of any baseline \$B\$ that does not depend on \$a\$ is given by:

```math
\begin{align}
\mathbb{E}_{a \sim \pi_1(a|s)} [B(s, b) \nabla_{\theta_1} \log \pi_1(a|s) ]
=
\sum_{a} \pi_1(a|s_t) B(s, b) \nabla_{\theta_1}  \log \pi_1(a|s)  \\
=
\sum_{a} B(s, b) \nabla_{\theta_1} \pi_1(a|s)
=
B(s, b) \nabla_{\theta_1}  \sum_{a}  \pi_1(a|s)
=
B(s, b) \nabla_{\theta_1}  1
=
0
\end{align}
```

Notice that:

```math
\begin{align}
\mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t}^\infty \gamma^{k-t} B(s_k,b_k)  \nabla_{\theta_1} \log \pi^1(a_k \mid s_k)  \Bigm | s_t, a\sim\pi_1(\cdot|s_t), b_t\right]\\
=
(\dots) \cdot  \mathbb{E}_{a \sim \pi_1(a|s)} [B(s, b) \nabla_{\theta_1} \log \pi_1(a|s) ]
=
0
\end{align}
```

and so the opponent shaping term

```math
\begin{align}
=\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}_t\,
        \mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t}^\infty \gamma^{k-t} A^2_k  \nabla_{\theta_1} \log \pi^1(a_k \mid s_k)  \Bigm | s_t, a\sim\pi_1(\cdot|s_t), b_t\right]
\Bigg] \\
=\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        A^{1}_t\,
        \mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t}^\infty \gamma^k A^2_k  \nabla_{\theta_1} \log \pi^1(a_k' \mid s_k')  \Bigm | s_t' =s_t, a'_t\sim\pi_1(\cdot|s_t), b'_t=b_t\right]
\Bigg] \\
\end{align}
```


## AdAlign Derivation (Without Inner Expectation)

```math
\beta \mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t}\,
        A^{1}(s_t,a_t,b_t)\,
        \nabla_{\theta_1}
        Q^{2}(s_t,a_t,b_t)
\Bigg]
```

```math
=\beta\,
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t+1}\,
        A^{1}_t\,
        \nabla_{\theta_1}
        \mathbb{E}_{s'}\!\bigl[\,
            V^{2}(s')\mid s_t,a_t,b_t
        \bigr]
\Bigg]
```

```math
=\beta\,
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
        \gamma^{t+1}\,
        A^{1}_t\,
		\mathbb{E}_{\tau' \sim \Pr^{\pi^1, \pi^2}_\mu} \left[ \sum_{k=t+1}^\infty \gamma^k A^2_k \left( \nabla_{\theta_1} \log \pi^1(a_k \mid s_k) + \underbrace{ \cancel{ \nabla_{\theta_1} \log \hat{\pi}^2(b_k \mid s_k) } }_{\text{Stop Gradient}}\right) \Bigm| s_t, a_t, b_t\right]
\Bigg]
```

```math
\approx\;
\beta\,
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \mathbb{E}_{\tau'\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
    \Bigl[
        \sum_{t=0}^{\infty}
        \sum_{k=t+1}^{\infty}
            \gamma^{t+k+1}\,
            A^{1}_t
            A^{2}_k\,
            \nabla_{\theta_1}\!
            \log\pi^{1}(a'_k\!\mid s'_k)
        \,\Bigm|\,
        s_t,a_t,b_t
    \Bigr]
\Bigg]
```

```math
\approx \beta\,
\mathbb{E}_{\tau\sim\Pr_{\mu}^{\pi^{1},\pi^{2}}}
\!\Bigg[
    \sum_{t=0}^{\infty}
    \sum_{k=t+1}^{\infty}
        \gamma^{t+k+1}\,
        A^{1}_t
        A^{2}_k\,
        \nabla_{\theta_1}\!
        \log\pi^{1}(a_k\!\mid s_k)
\Bigg].
```

by [Law of Total Expectation](https://en.wikipedia.org/wiki/Law_of_total_expectation).

## Opponent Shaping Interpretation (Alternative to Stop Gradient)

Instead of having a stop gradient, we can soften the assumptions. In short, Policy \$i\$ assumes that Policy \$j\$ is a Boltzmann one-step lookahead policy where the future fixed policy \$\pi^j\_\text{fixed}\$ is the one used to sample the rollouts.

More precisely, from the perspective of \$\pi\_1\$, it is assumed that:

```math
\hat{\pi}^2(b_t \mid a_t, s_t) := \frac{\exp \tilde{Q}^2(s_t, a_t, b_t)}{\sum_b \exp \tilde{Q}^2(s_t, a_t, b_t)}
```

where:

```math
\tilde{Q}^2(s_t, a_t, b_t) := r^2(s_t, a_t, b_t) + \gamma \cdot \mathbb{E}_{s_{t+1}} \left[ V^2_{\theta_1}(s_{t+1}) \right]
```

and:

```math
V^2_{\theta_1}(\mu) := \mathbb{E}_{\tau \sim \Pr_\mu^{\pi^1_{\theta_1}, \pi^2_{\text{fixed}}}} \left[ \sum_{k=0}^{\infty} \gamma^k r^2_k \right]
```

During the sampling of trajectories, \$\pi^2\_{\text{fixed}}\$ becomes the actual policy of the other agent.

