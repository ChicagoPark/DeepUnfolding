# DeepUnfolding


Loss
```python
if module.name == 'DEQ':
    x_hat, forward_iter, forward_res = module(x_init, P, S, y)
else:
    x_hat = module(x_init, P, S, y)

loss = loss_fn(torch.view_as_real(x_hat), torch.view_as_real(x))

```

PSNR
```python
x_hat = torch.abs(x_hat)
x_hat = (x_hat - torch.min(x_hat)) / (torch.max(x_hat) - torch.min(x_hat))

x = torch.abs(x)
x = (x - torch.min(x)) / (torch.max(x) - torch.min(x))
```
