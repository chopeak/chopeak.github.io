```c#
namespace SmartLabelPrint
{
    partial class MainForm
    {
        private System.ComponentModel.IContainer components = null;

​```
    // ==================== 控件声明 ====================
    private System.Windows.Forms.Panel panelSidebar;
    private System.Windows.Forms.Panel panelLogo;
    private System.Windows.Forms.Label lblLogo;
    private System.Windows.Forms.Button btnPrint;
    private System.Windows.Forms.Button btnMaterial;
    private System.Windows.Forms.Button btnCustomer;
    private System.Windows.Forms.Button btnOrder;
    private System.Windows.Forms.Button btnField;
    private System.Windows.Forms.Button btnPrinter;
    private System.Windows.Forms.Button btnHistory;
    private System.Windows.Forms.Button btnHelp;
    private System.Windows.Forms.Button btnAbout;
    private System.Windows.Forms.Panel panelContent;
    private System.Windows.Forms.Label lblPageTitle;
    private System.Windows.Forms.StatusStrip statusStrip;
    private System.Windows.Forms.ToolStripStatusLabel lblStatus;
    private System.Windows.Forms.ToolStripStatusLabel lblUser;
    private System.Windows.Forms.ToolStripStatusLabel lblTime;

    private void InitializeComponent()
    {
        this.components = new System.ComponentModel.Container();

        // ===== 窗体 =====
        this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
        this.ClientSize = new System.Drawing.Size(1000, 750);

        // ===== 1. 侧边栏 =====
        this.panelSidebar = new System.Windows.Forms.Panel();
        this.panelSidebar.Dock = System.Windows.Forms.DockStyle.Left;
        this.panelSidebar.Width = 150;
        this.panelSidebar.BackColor = System.Drawing.Color.White;

        // ===== Logo =====
        this.panelLogo = new System.Windows.Forms.Panel();
        this.panelLogo.Dock = System.Windows.Forms.DockStyle.Top;
        this.panelLogo.Height = 56;
        this.panelLogo.BackColor = System.Drawing.Color.FromArgb(44, 62, 80);

        this.lblLogo = new System.Windows.Forms.Label();
        this.lblLogo.Text = "🏷️ SmartLabel";
        this.lblLogo.Font = new System.Drawing.Font("微软雅黑", 12, System.Drawing.FontStyle.Bold);
        this.lblLogo.ForeColor = System.Drawing.Color.White;
        this.lblLogo.Dock = System.Windows.Forms.DockStyle.Fill;
        this.lblLogo.TextAlign = System.Drawing.ContentAlignment.MiddleCenter;
        this.panelLogo.Controls.Add(this.lblLogo);

        this.panelSidebar.Controls.Add(this.panelLogo);

        // ===== 导航按钮 =====
        string[] navItems = { 
            "🏷️ 标签打印", "📦 物料信息", "👤 客户信息", 
            "📋 订单信息", "📐 字段模板", "🖨️ 打印机管理", 
            "📊 打印历史", "❓ 帮助文档", "ℹ️ 关于" 
        };
        string[] navTags = { 
            "标签打印", "物料信息", "客户信息", "订单信息", 
            "字段模板", "打印机管理", "打印历史", "帮助文档", "关于" 
        };

        for (int i = 0; i < navItems.Length; i++)
        {
            System.Windows.Forms.Button btn = new System.Windows.Forms.Button();
            btn.Text = navItems[i];
            btn.Tag = navTags[i];
            btn.Dock = System.Windows.Forms.DockStyle.Top;
            btn.Height = 36;
            btn.FlatStyle = System.Windows.Forms.FlatStyle.Flat;
            btn.FlatAppearance.BorderSize = 0;
            btn.BackColor = System.Drawing.Color.Transparent;
            btn.TextAlign = System.Drawing.ContentAlignment.MiddleLeft;
            btn.Padding = new System.Windows.Forms.Padding(12, 0, 0, 0);
            btn.Font = new System.Drawing.Font("微软雅黑", 9);
            btn.Cursor = System.Windows.Forms.Cursors.Hand;

            if (i == 0) this.btnPrint = btn;
            else if (i == 1) this.btnMaterial = btn;
            else if (i == 2) this.btnCustomer = btn;
            else if (i == 3) this.btnOrder = btn;
            else if (i == 4) this.btnField = btn;
            else if (i == 5) this.btnPrinter = btn;
            else if (i == 6) this.btnHistory = btn;
            else if (i == 7) this.btnHelp = btn;
            else if (i == 8) this.btnAbout = btn;

            this.panelSidebar.Controls.Add(btn);
        }

        // ===== 2. 内容区域 =====
        this.panelContent = new System.Windows.Forms.Panel();
        this.panelContent.Dock = System.Windows.Forms.DockStyle.Fill;
        this.panelContent.BackColor = System.Drawing.Color.White;
        this.panelContent.Padding = new System.Windows.Forms.Padding(8, 4, 8, 4);

        // ===== 标题栏 =====
        this.lblPageTitle = new System.Windows.Forms.Label();
        this.lblPageTitle.Text = "标签打印";
        this.lblPageTitle.Font = new System.Drawing.Font("微软雅黑", 11, System.Drawing.FontStyle.Bold);
        this.lblPageTitle.ForeColor = System.Drawing.Color.FromArgb(52, 73, 94);
        this.lblPageTitle.Dock = System.Windows.Forms.DockStyle.Top;
        this.lblPageTitle.Height = 28;
        this.panelContent.Controls.Add(this.lblPageTitle);

        // ===== 3. 状态栏 =====
        this.statusStrip = new System.Windows.Forms.StatusStrip();
        this.statusStrip.BackColor = System.Drawing.Color.FromArgb(250, 250, 250);
        this.statusStrip.SizingGrip = false;

        this.lblStatus = new System.Windows.Forms.ToolStripStatusLabel();
        this.lblStatus.Text = "就绪";
        this.lblStatus.Spring = true;
        this.lblStatus.TextAlign = System.Drawing.ContentAlignment.MiddleLeft;
        this.statusStrip.Items.Add(this.lblStatus);

        this.lblUser = new System.Windows.Forms.ToolStripStatusLabel();
        this.lblUser.Text = "用户: 张工";
        this.statusStrip.Items.Add(this.lblUser);

        this.lblTime = new System.Windows.Forms.ToolStripStatusLabel();
        this.lblTime.Text = System.DateTime.Now.ToString("yyyy-MM-dd HH:mm");
        this.statusStrip.Items.Add(this.lblTime);

        // ===== 添加到窗体 =====
        this.Controls.Add(this.panelContent);
        this.Controls.Add(this.panelSidebar);
        this.Controls.Add(this.statusStrip);

        // ===== 绑定导航事件 =====
        this.btnPrint.Click += (s, e) => SwitchPage("标签打印");
        this.btnMaterial.Click += (s, e) => SwitchPage("物料信息");
        this.btnCustomer.Click += (s, e) => SwitchPage("客户信息");
        this.btnOrder.Click += (s, e) => SwitchPage("订单信息");
        this.btnField.Click += (s, e) => SwitchPage("字段模板");
        this.btnPrinter.Click += (s, e) => SwitchPage("打印机管理");
        this.btnHistory.Click += (s, e) => SwitchPage("打印历史");
        this.btnHelp.Click += (s, e) => SwitchPage("帮助文档");
        this.btnAbout.Click += (s, e) => SwitchPage("关于");
    }

    protected override void Dispose(bool disposing)
    {
        if (disposing && (components != null))
        {
            components.Dispose();
        }
        base.Dispose(disposing);
    }
}
​```

}
```

