# KN-222
ImageProcessing:
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

namespace ImageProcessing
{
    public partial class ImageProcessing : Form
    {
        OpenFileDialog oDlg;
        SaveFileDialog sDlg;
        double zoomFactor = 1.0;
        private MenuItem cZoom;
        ImageHandler imageHandler = new ImageHandler();

        public ImageProcessing()
        {
            InitializeComponent();
            oDlg = new OpenFileDialog(); // Open Dialog Initialization
            oDlg.RestoreDirectory = true;
            oDlg.InitialDirectory = "C:\\";
            oDlg.FilterIndex = 1;
            oDlg.Filter = "jpg Files (*.jpg)|*.jpg|gif Files (*.gif)|*.gif|png Files (*.png)|*.png |bmp Files (*.bmp)|*.bmp";
            /*************************/
            sDlg = new SaveFileDialog(); // Save Dialog Initialization
            sDlg.RestoreDirectory = true;
            sDlg.InitialDirectory = "C:\\";
            sDlg.FilterIndex = 1;
            sDlg.Filter = "jpg Files (*.jpg)|*.jpg|gif Files (*.gif)|*.gif|png Files (*.png)|*.png |bmp Files (*.bmp)|*.bmp";
            /*************************/
            cZoom = menuItemZoom100; // Current Zoom Percentage = 100%
        }

        private void ImageProcessing_Paint(object sender, PaintEventArgs e)
        {
            Graphics g = e.Graphics;
            g.DrawImage(imageHandler.CurrentBitmap, new Rectangle(this.AutoScrollPosition.X, this.AutoScrollPosition.Y, Convert.ToInt32(imageHandler.CurrentBitmap.Width * zoomFactor), Convert.ToInt32(imageHandler.CurrentBitmap.Height * zoomFactor)));
        }

        private void menuItemOpen_Click(object sender, EventArgs e)
        {
            if (DialogResult.OK == oDlg.ShowDialog())
            {
                imageHandler.CurrentBitmap = (Bitmap)Bitmap.FromFile(oDlg.FileName);
                imageHandler.BitmapPath = oDlg.FileName;
                this.AutoScroll = true;
                this.AutoScrollMinSize = new Size(Convert.ToInt32(imageHandler.CurrentBitmap.Width * zoomFactor), Convert.ToInt32(imageHandler.CurrentBitmap.Height * zoomFactor));
                this.Invalidate();
                menuItemImageInfo.Enabled = true;
                ImageInfo imgInfo = new ImageInfo(imageHandler);
                imgInfo.Show();
            }
        }

        private void menuItemSave_Click(object sender, EventArgs e)
        {
            if (DialogResult.OK == sDlg.ShowDialog())
            {
                imageHandler.SaveBitmap(sDlg.FileName);
            }
        }

        private void menuItemExit_Click(object sender, EventArgs e)
        {
            this.Close();
        }
InsertTextForm itFrm = new InsertTextForm();
            itFrm.XPosition = itFrm.YPosition = 0;
            if (itFrm.ShowDialog() == DialogResult.OK)
            {
                imageHandler.RestorePrevious();
                imageHandler.InsertText(itFrm.DisplayText, itFrm.XPosition, itFrm.YPosition, itFrm.DisplayTextFont, itFrm.DisplayTextFontSize, itFrm.DisplayTextFontStyle, itFrm.DisplayTextForeColor1, itFrm.DisplayTextForeColor2);
                this.Invalidate();
            }
        }

        private void menuItemInsertImage_Click(object sender, EventArgs e)
        {
            InsertImageForm iiFrm = new InsertImageForm();
            iiFrm.XPosition = iiFrm.YPosition = 0;
            if (iiFrm.ShowDialog() == DialogResult.OK)
            {
                imageHandler.RestorePrevious();
                imageHandler.InsertImage(iiFrm.DisplayImagePath, iiFrm.XPosition, iiFrm.YPosition);
                this.Invalidate();
            }
        }

        private void menuItemInsertShape_Click(object sender, EventArgs e)
        {
            InsertShapeForm isFrm = new InsertShapeForm();
            isFrm.XPosition = isFrm.YPosition = 0;
            if (isFrm.ShowDialog() == DialogResult.OK)
            {
                imageHandler.RestorePrevious();
                imageHandler.InsertShape(isFrm.ShapeType, isFrm.XPosition, isFrm.YPosition, isFrm.ShapeWidth, isFrm.ShapeHeight, isFrm.ShapeColor);
                this.Invalidate();
            }
        }
    }
}

3.2 ДОДАТОК Б

Insert Image:

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

namespace ImageProcessing
{
    public partial class InsertImageForm : Form
    {
        OpenFileDialog oDlg;
        public InsertImageForm()
        {
            InitializeComponent();
            oDlg = new OpenFileDialog(); // Open Dialog Initialization
            oDlg.RestoreDirectory = true;
            oDlg.InitialDirectory = "C:\\";
            oDlg.FilterIndex = 1;
            oDlg.Filter = "jpg Files (*.jpg)|*.jpg|gif Files (*.gif)|*.gif|png Files (*.png)|*.png |bmp Files (*.bmp)|*.bmp";
            btnOK.DialogResult = DialogResult.OK;
            btnCancel.DialogResult = DialogResult.Cancel;
        }

        public int XPosition
        {
            get 
            {
                if (string.IsNullOrEmpty(txtX.Text))
                    txtX.Text = "0";
                return Convert.ToInt32(txtX.Text); 
            }
            set { txtX.Text = value.ToString(); }
        }

        public int YPosition
        {
            get
            {
                if (string.IsNullOrEmpty(txtY.Text))
                    txtY.Text = "0";
                return Convert.ToInt32(txtY.Text);
            }
            set { txtY.Text = value.ToString(); }
        }

        public string DisplayImagePath
        {
            get { return txtImage.Text; }
            set { txtImage.Text = value.ToString(); }
        }

        private void btnSelect_Click(object sender, EventArgs e)
        {
            if (DialogResult.OK == oDlg.ShowDialog())
            {
                txtImage.Text = oDlg.FileName;
            }
        }
    }
}

Insert Text:

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

namespace ImageProcessing
{
    public partial class InsertTextForm : Form
    {
        public InsertTextForm()
        {
            InitializeComponent();
            btnOK.DialogResult = DialogResult.OK;
            btnCancel.DialogResult = DialogResult.Cancel;
        }

        public int XPosition
        {
            get
            {
                if (string.IsNullOrEmpty(txtX.Text))
                    txtX.Text = "0";
                return Convert.ToInt32(txtX.Text);
            }
            set { txtX.Text = value.ToString(); }
        }

        public int YPosition
        {
            get
            {
                if (string.IsNullOrEmpty(txtY.Text))
                    txtY.Text = "0";
                return Convert.ToInt32(txtY.Text);
            }
            set { txtY.Text = value.ToString(); }
        }

        public string DisplayText
        {
            get { return txtText.Text; }
            set { txtText.Text = value.ToString(); }
        }

        public string DisplayTextFont
        {
            get { return cmbFonts.Text; }
            set { cmbFonts.Text = value.ToString(); }
        }

        public float DisplayTextFontSize
        {
            get
            {
                float fs = 10.0F;
                if (!string.IsNullOrEmpty(cmbFontSize.Text))
                    fs = Convert.ToSingle(cmbFontSize.Text.Replace("pt", ""));
                return fs;
            }
            set { cmbFonts.Text = value.ToString() + "pt"; }
        }

        public string DisplayTextFontStyle
        {
            get { return cmbFontStyles.Text; }
            set { cmbFontStyles.Text = value.ToString(); }
        }

        public string DisplayTextForeColor1
        {
            get { return cmbColors1.Text; }
            set { cmbColors1.Text = value.ToString(); }
        }

        public string DisplayTextForeColor2
        {
            get { return cmbColors2.Text; }
            set { cmbColors2.Text = value.ToString(); }
        }

        private void InsertTextForm_Load(object sender, EventArgs e)
        {
            // Load Fonts.
            foreach (FontFamily ff in FontFamily.Families)
            {
                cmbFonts.Items.Add(ff.Name);
            }
            // Load Font Size.
            for (int i = 5; i <= 75; i += 5)
            {
                cmbFontSize.Items.Add(i.ToString() + "pt");
            }
            // Load Font Styles.
            cmbFontStyles.Items.Add("Bold");
            cmbFontStyles.Items.Add("Italic");
            cmbFontStyles.Items.Add("Regular");
            cmbFontStyles.Items.Add("Strikeout");
            cmbFontStyles.Items.Add("Underline");
            // Load Colors.
            Type type = typeof(System.Drawing.Color);
            System.Reflection.PropertyInfo[] propertyInfo = type.GetProperties();
            for (int i = 0; i < propertyInfo.Length; i++)
            {
                if (propertyInfo[i].Name != "Transparent"
                    && propertyInfo[i].Name != "R"
                    && propertyInfo[i].Name != "G"
                    && propertyInfo[i].Name != "B"
                    && propertyInfo[i].Name != "A"
                    && propertyInfo[i].Name != "IsKnownColor"
                    && propertyInfo[i].Name != "IsEmpty"
                    && propertyInfo[i].Name != "IsNamedColor"
                    && propertyInfo[i].Name != "IsSystemColor"
                    && propertyInfo[i].Name != "Name")
                {
                    cmbColors1.Items.Add(propertyInfo[i].Name);
                    cmbColors2.Items.Add(propertyInfo[i].Name);
                }
            }
        }

        private void GradientCheck_CheckedChanged(object sender, EventArgs e)
        {
            cmbColors2.Enabled = GradientCheck.Checked;
        }
    }
}

Insert Shape:

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

namespace ImageProcessing
{
    public partial class InsertShapeForm : Form
    {
        public InsertShapeForm()
        {
            InitializeComponent();
            btnOK.DialogResult = DialogResult.OK;
            btnCancel.DialogResult = DialogResult.Cancel;
        }

        public int XPosition
        {
            get
            {
                if (string.IsNullOrEmpty(txtX.Text))
                    txtX.Text = "0";
                return Convert.ToInt32(txtX.Text);
            }
            set { txtX.Text = value.ToString(); }
        }

        public int YPosition
        {
            get
            {
                if (string.IsNullOrEmpty(txtY.Text))
                    txtY.Text = "0";
                return Convert.ToInt32(txtY.Text);
            }
            set { txtY.Text = value.ToString(); }
        }

        public int ShapeWidth
        {
            get
            {
                if (string.IsNullOrEmpty(txtWidth.Text))
                    txtWidth.Text = "0";
                return Convert.ToInt32(txtWidth.Text);
            }
            set { txtWidth.Text = value.ToString(); }
        }

        public int ShapeHeight
        {
            get
            {
                if (string.IsNullOrEmpty(txtHeight.Text))
                    txtHeight.Text = "0";
                return Convert.ToInt32(txtHeight.Text);
            }
            set { txtHeight.Text = value.ToString(); }
        }

        public string ShapeType
        {
            get { return cmbShapes.Text; }
            set { cmbShapes.Text = value.ToString(); }
        }

        public string ShapeColor
        {
            get { return cmbColors.Text; }
            set { cmbColors.Text = value.ToString(); }
        }

        private void InsertShapeForm_Load(object sender, EventArgs e)
        {
            // Load Shapes.
            cmbShapes.Items.Add("FilledEllipse");
            cmbShapes.Items.Add("FilledRectangle");
            cmbShapes.Items.Add("Ellipse");
            cmbShapes.Items.Add("Rectangle");
            // Load Colors.
            Type type = typeof(System.Drawing.Color);
            System.Reflection.PropertyInfo[] propertyInfo = type.GetProperties();
            for (int i = 0; i < propertyInfo.Length; i++)
            {
                if (propertyInfo[i].Name != "Transparent"
                    && propertyInfo[i].Name != "R"
                    && propertyInfo[i].Name != "G"
                    && propertyInfo[i].Name != "B"
                    && propertyInfo[i].Name != "A"
                    && propertyInfo[i].Name != "IsKnownColor"
                    && propertyInfo[i].Name != "IsEmpty"
                    && propertyInfo[i].Name != "IsNamedColor"
                    && propertyInfo[i].Name != "IsSystemColor"
                    && propertyInfo[i].Name != "Name")
                {
                    cmbColors.Items.Add(propertyInfo[i].Name);
                }
            }
        }
    }
}



