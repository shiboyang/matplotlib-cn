# 演示Axes Divider

轴分割器用于计算轴的位置，并使用现有轴实例为它们创建分隔线。

![演示Axes Divider示例](/static/images/gallery/sphx_glr_demo_axes_divider_001.png)

```python
import matplotlib.pyplot as plt


def get_demo_image():
    import numpy as np
    from matplotlib.cbook import get_sample_data
    f = get_sample_data("axes_grid/bivariate_normal.npy", asfileobj=False)
    z = np.load(f)
    # z is a numpy array of 15x15
    return z, (-3, 4, -4, 3)


def demo_simple_image(ax):
    Z, extent = get_demo_image()

    im = ax.imshow(Z, extent=extent, interpolation="nearest")
    cb = plt.colorbar(im)
    plt.setp(cb.ax.get_yticklabels(), visible=False)


def demo_locatable_axes_hard(fig1):

    from mpl_toolkits.axes_grid1 import SubplotDivider, Size
    from mpl_toolkits.axes_grid1.mpl_axes import Axes

    divider = SubplotDivider(fig1, 2, 2, 2, aspect=True)

    # axes for image
    ax = Axes(fig1, divider.get_position())

    # axes for colorbar
    ax_cb = Axes(fig1, divider.get_position())

    h = [Size.AxesX(ax),  # main axes
         Size.Fixed(0.05),  # padding, 0.1 inch
         Size.Fixed(0.2),  # colorbar, 0.3 inch
         ]

    v = [Size.AxesY(ax)]

    divider.set_horizontal(h)
    divider.set_vertical(v)

    ax.set_axes_locator(divider.new_locator(nx=0, ny=0))
    ax_cb.set_axes_locator(divider.new_locator(nx=2, ny=0))

    fig1.add_axes(ax)
    fig1.add_axes(ax_cb)

    ax_cb.axis["left"].toggle(all=False)
    ax_cb.axis["right"].toggle(ticks=True)

    Z, extent = get_demo_image()

    im = ax.imshow(Z, extent=extent, interpolation="nearest")
    plt.colorbar(im, cax=ax_cb)
    plt.setp(ax_cb.get_yticklabels(), visible=False)


def demo_locatable_axes_easy(ax):
    from mpl_toolkits.axes_grid1 import make_axes_locatable

    divider = make_axes_locatable(ax)

    ax_cb = divider.new_horizontal(size="5%", pad=0.05)
    fig1 = ax.get_figure()
    fig1.add_axes(ax_cb)

    Z, extent = get_demo_image()
    im = ax.imshow(Z, extent=extent, interpolation="nearest")

    plt.colorbar(im, cax=ax_cb)
    ax_cb.yaxis.tick_right()
    ax_cb.yaxis.set_tick_params(labelright=False)


def demo_images_side_by_side(ax):
    from mpl_toolkits.axes_grid1 import make_axes_locatable

    divider = make_axes_locatable(ax)

    Z, extent = get_demo_image()
    ax2 = divider.new_horizontal(size="100%", pad=0.05)
    fig1 = ax.get_figure()
    fig1.add_axes(ax2)

    ax.imshow(Z, extent=extent, interpolation="nearest")
    ax2.imshow(Z, extent=extent, interpolation="nearest")
    ax2.yaxis.set_tick_params(labelleft=False)


def demo():

    fig1 = plt.figure(1, (6, 6))
    fig1.clf()

    # PLOT 1
    # simple image & colorbar
    ax = fig1.add_subplot(2, 2, 1)
    demo_simple_image(ax)

    # PLOT 2
    # image and colorbar whose location is adjusted in the drawing time.
    # a hard way

    demo_locatable_axes_hard(fig1)

    # PLOT 3
    # image and colorbar whose location is adjusted in the drawing time.
    # a easy way

    ax = fig1.add_subplot(2, 2, 3)
    demo_locatable_axes_easy(ax)

    # PLOT 4
    # two images side by side with fixed padding.

    ax = fig1.add_subplot(2, 2, 4)
    demo_images_side_by_side(ax)

    plt.show()


demo()
```

## 下载这个示例
            
- [下载python源码: demo_axes_divider.py](https://matplotlib.org/_downloads/demo_axes_divider.py)
- [下载Jupyter notebook: demo_axes_divider.ipynb](https://matplotlib.org/_downloads/demo_axes_divider.ipynb)