# DeepUnfolding


Done | PLAN
:--------------- | :--------------- 
Done   | Check the generated dataset from DeCoLearn
Done   | Check the size of the tensor of generated dataset
Done      | Plan how to compose dataset inside
4      | Separate Directory for Multi-coli system
4-1    | 
5      | Visualize 
     


[Original DeCoLearn dataset organization]

source_h5  	      
:---------------: 
x (trnOrg + tstOrg)  

alignment_h5  	      
:---------------: 
moved_x

mri_h5  	      
:---------------: 
moved_x
fixed_y
fixed_mask
fixed_y_tran
moved_y
moved_mask
moved_y_tran
moved_y_warped_truth
moved_y_tran_warped_truth


> <img width="900" alt="IMG" src="https://user-images.githubusercontent.com/73331241/180142212-3dc92e7c-7e93-4be5-876f-7a86cd8e21d6.png">

[Planned dataset organization]
source_h5  	      
:---------------: 
x (trnOrg + tstOrg) 
s (trnCsm + tstOrg)

alignment_h5  	      
:---------------: 
moved_x

mri_h5  	      
:---------------: 
moved_x
fixed_y
fixed_mask
fixed_y_tran
moved_y
moved_mask
moved_y_tran
moved_y_warped_truth
moved_y_tran_warped_truth




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

Mistake
1. Put measurement y into the CNN (I should have put measurement y into loss function but it was my mistake.)

Dimentia
1. to_tiff is visualiation method
2. numpy.imag(val) return imaginary part of complex value.


Question
1. How do you know to use the code below to visualize MRI image at the super first time.
2. Tip in large scale code
3. Reason of the batch size 1
4. How do you train deep learning model. Do you use your own GPU or Colab?
5. Why did you multiply mask before estimating loss function?
6. models - learning figures from Kamilov lecture

```python
to_tiff(torch.sqrt(moved_x[:, 0] ** 2 + moved_x[:, 1] ** 2), path=alignment_qc + 'moved_x.tiff',
                    is_normalized=False)
```

