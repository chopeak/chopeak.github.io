```c#
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;

namespace SmartLabelPrint
{
    public partial class MainForm : Form
    {
        private Dictionary<string, Panel> _pages = new Dictionary<string, Panel>();

​```
    public MainForm()
    {
        InitializeComponent();
        
        // 窗体设置
        this.Text = "SmartLabelPrint v2.0 - 智能标签打印系统";
        this.Size = new Size(1000, 750);
        this.StartPosition = FormStartPosition.CenterScreen;
        this.FormBorderStyle = FormBorderStyle.FixedSingle;
        this.MaximizeBox = false;
        this.BackColor = Color.FromArgb(240, 242, 245);
        this.KeyPreview = true;

        // 初始化页面
        InitializePages();
        
        // 默认显示标签打印
        SwitchPage("标签打印");
        
        // 更新时间
        UpdateTime();
    }

    private void InitializePages()
    {
        // 创建所有页面面板
        _pages["标签打印"] = CreatePrintPanel();
        _pages["物料信息"] = CreateMaterialPanel();
        _pages["客户信息"] = CreateCustomerPanel();
        _pages["订单信息"] = CreateOrderPanel();
        _pages["字段模板"] = CreateFieldPanel();
        _pages["打印机管理"] = CreatePrinterPanel();
        _pages["打印历史"] = CreateHistoryPanel();
        _pages["帮助文档"] = CreateHelpPanel();
        _pages["关于"] = CreateAboutPanel();

        // 添加到内容区域
        foreach (var page in _pages.Values)
        {
            page.Dock = DockStyle.Fill;
            page.Visible = false;
            panelContent.Controls.Add(page);
        }
    }

    private void SwitchPage(string pageName)
    {
        // 隐藏所有页面
        foreach (var page in _pages.Values)
        {
            page.Visible = false;
        }

        // 显示目标页面
        if (_pages.TryGetValue(pageName, out Panel target))
        {
            target.Visible = true;
            target.BringToFront();
        }

        // 更新标题
        lblPageTitle.Text = pageName;

        // 高亮导航按钮
        HighlightNavButton(pageName);

        // 更新状态
        lblStatus.Text = $"当前: {pageName}";
    }

    private void HighlightNavButton(string pageName)
    {
        foreach (Control ctrl in panelSidebar.Controls)
        {
            if (ctrl is Button btn && btn.Tag != null)
            {
                if (btn.Tag.ToString() == pageName)
                {
                    btn.BackColor = Color.FromArgb(52, 152, 219);
                    btn.ForeColor = Color.White;
                }
                else
                {
                    btn.BackColor = Color.Transparent;
                    btn.ForeColor = Color.FromArgb(60, 60, 60);
                }
            }
        }
    }

    private void UpdateTime()
    {
        var timer = new Timer { Interval = 10000 };
        timer.Tick += (s, e) =>
        {
            if (statusStrip.Items.Count > 2)
            {
                statusStrip.Items[2].Text = DateTime.Now.ToString("yyyy-MM-dd HH:mm");
            }
        };
        timer.Start();
        statusStrip.Items[2].Text = DateTime.Now.ToString("yyyy-MM-dd HH:mm");
    }

    // ==================== 页面创建方法 ====================

    private Panel CreatePrintPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };

        // 标题
        var title = new Label
        {
            Text = "📋 标签打印",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        };
        panel.Controls.Add(title);

        // 选择行
        var selectionPanel = new FlowLayoutPanel
        {
            Dock = DockStyle.Top,
            Height = 36,
            Padding = new Padding(0, 4, 0, 4),
            FlowDirection = FlowDirection.LeftToRight,
            WrapContents = false
        };

        // 客户
        selectionPanel.Controls.Add(CreateLabel("客户:"));
        var cmbCustomer = new ComboBox { Width = 120, Font = new Font("微软雅黑", 8.5f), DropDownStyle = ComboBoxStyle.DropDownList };
        cmbCustomer.Items.AddRange(new object[] { "华为技术有限公司", "中兴通讯股份有限公司", "小米科技有限责任公司" });
        cmbCustomer.SelectedIndex = 0;
        selectionPanel.Controls.Add(cmbCustomer);

        // 标签类型
        selectionPanel.Controls.Add(CreateLabel("标签:"));
        var cmbLabelType = new ComboBox { Width = 90, Font = new Font("微软雅黑", 8.5f), DropDownStyle = ComboBoxStyle.DropDownList };
        cmbLabelType.Items.AddRange(new object[] { "内袋", "外箱", "通用" });
        cmbLabelType.SelectedIndex = 0;
        selectionPanel.Controls.Add(cmbLabelType);

        // 打印机
        selectionPanel.Controls.Add(CreateLabel("打印机:"));
        var cmbPrinter = new ComboBox { Width = 150, Font = new Font("微软雅黑", 8.5f), DropDownStyle = ComboBoxStyle.DropDownList };
        try
        {
            cmbPrinter.Items.AddRange(System.Drawing.Printing.PrinterSettings.InstalledPrinters);
            if (cmbPrinter.Items.Count > 0) cmbPrinter.SelectedIndex = 0;
        }
        catch { }
        selectionPanel.Controls.Add(cmbPrinter);

        // 份数
        selectionPanel.Controls.Add(CreateLabel("份数:"));
        var nudCopies = new NumericUpDown { Width = 50, Minimum = 1, Maximum = 999, Value = 1 };
        selectionPanel.Controls.Add(nudCopies);

        // 打印按钮
        var btnPrint = new Button
        {
            Text = "🖨️ 打印",
            Font = new Font("微软雅黑", 8.5f, FontStyle.Bold),
            BackColor = Color.FromArgb(155, 89, 182),
            ForeColor = Color.White,
            Size = new Size(80, 26),
            FlatStyle = FlatStyle.Flat,
            FlatAppearance = { BorderSize = 0 },
            Cursor = Cursors.Hand
        };
        btnPrint.Click += (s, e) => MessageBox.Show("打印功能开发中", "提示");
        selectionPanel.Controls.Add(btnPrint);

        panel.Controls.Add(selectionPanel);

        // 主体：左右分割
        var split = new SplitContainer
        {
            Dock = DockStyle.Fill,
            Orientation = Orientation.Vertical,
            SplitterDistance = 380,
            SplitterWidth = 4,
            BackColor = Color.FromArgb(224, 224, 224)
        };

        // 左侧：录入区
        var leftPanel = CreateInputPanel();
        split.Panel1.Controls.Add(leftPanel);

        // 右侧：预览区
        var rightPanel = CreatePreviewPanel();
        split.Panel2.Controls.Add(rightPanel);

        panel.Controls.Add(split);

        return panel;
    }

    private Panel CreateInputPanel()
    {
        var panel = new Panel { Padding = new Padding(6, 2, 6, 2), BackColor = Color.White };

        var lblTitle = new Label
        {
            Text = "📝 数据录入",
            Font = new Font("微软雅黑", 9, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 22
        };
        panel.Controls.Add(lblTitle);

        var table = new TableLayoutPanel
        {
            Dock = DockStyle.Fill,
            ColumnCount = 2,
            RowCount = 13,
            Padding = new Padding(0, 24, 0, 2),
            BackColor = Color.Transparent
        };
        table.ColumnStyles.Add(new ColumnStyle(SizeType.Absolute, 75));
        table.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 100));

        // 控件
        var controls = new (string label, Control control)[]
        {
            ("订单号:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("物料编码:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("产品名称:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("规格:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("包规(每袋):", new NumericUpDown { Dock = DockStyle.Fill, Minimum = 0, Maximum = 99999 }),
            ("箱规(每箱):", new NumericUpDown { Dock = DockStyle.Fill, Minimum = 0, Maximum = 99999 }),
            ("材质:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("工艺:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("版本号:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("英文描述:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("数量:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f), Text = "0" }),
            ("批次号:", new TextBox { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f) }),
            ("日期:", new DateTimePicker { Dock = DockStyle.Fill, Font = new Font("微软雅黑", 8.5f), Format = DateTimePickerFormat.Short, Value = DateTime.Now })
        };

        for (int i = 0; i < controls.Length; i++)
        {
            table.Controls.Add(new Label { Text = controls[i].label, Font = new Font("微软雅黑", 8.5f), TextAlign = ContentAlignment.MiddleRight, Dock = DockStyle.Fill }, 0, i);
            table.Controls.Add(controls[i].control, 1, i);
        }

        panel.Controls.Add(table);
        return panel;
    }

    private Panel CreatePreviewPanel()
    {
        var panel = new Panel { Padding = new Padding(4), BackColor = Color.White };

        var lblTitle = new Label
        {
            Text = "🔍 实时预览",
            Font = new Font("微软雅黑", 9, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 22
        };
        panel.Controls.Add(lblTitle);

        var picPreview = new PictureBox
        {
            Dock = DockStyle.Fill,
            SizeMode = PictureBoxSizeMode.Zoom,
            BackColor = Color.White,
            Margin = new Padding(0, 24, 0, 0)
        };
        panel.Controls.Add(picPreview);

        var hint = new Label
        {
            Text = "📷 输入数据后自动预览",
            Font = new Font("微软雅黑", 14),
            ForeColor = Color.Gray,
            Dock = DockStyle.Fill,
            TextAlign = ContentAlignment.MiddleCenter,
            BackColor = Color.White
        };
        panel.Controls.Add(hint);
        hint.BringToFront();

        return panel;
    }

    private Panel CreateMaterialPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "📦 物料信息",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        // ... 后续添加 DataGridView
        return panel;
    }

    private Panel CreateCustomerPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "👤 客户信息",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Panel CreateOrderPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "📋 订单信息",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Panel CreateFieldPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "📐 字段模板",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Panel CreatePrinterPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "🖨️ 打印机管理",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Panel CreateHistoryPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "📊 打印历史",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Panel CreateHelpPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "❓ 帮助文档",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Panel CreateAboutPanel()
    {
        var panel = new Panel { Padding = new Padding(8, 4, 8, 4), BackColor = Color.White };
        panel.Controls.Add(new Label
        {
            Text = "ℹ️ 关于",
            Font = new Font("微软雅黑", 10, FontStyle.Bold),
            ForeColor = Color.FromArgb(52, 73, 94),
            Dock = DockStyle.Top,
            Height = 28
        });
        return panel;
    }

    private Label CreateLabel(string text)
    {
        return new Label
        {
            Text = text,
            Font = new Font("微软雅黑", 8.5f),
            ForeColor = Color.FromArgb(60, 60, 60),
            AutoSize = true,
            Margin = new Padding(0, 4, 2, 4)
        };
    }
}
​```

}
```

