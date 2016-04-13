// Project-HTML
// Make HTML for Students Information System
// Make First Version on March 30,2016
/*
1512586
Le Thi Thien Trang
13-04-2016
Xu ly File HTML
*/

// Khai bao thu vien su dung
#pragma warning (disable:4996)
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
using namespace std;

// Khai bao cau truc sinh vien
struct SINHVIEN
{
	string mssv, ten, khoa, nienkhoa, ngaysinh, hinhanh, mota, *sothich;
};

// Khai bao ham
void XuLyThongTin(string SinhVien, SINHVIEN sv[], int dem, string *&luu_SinhVien, int dem_SV, int *&ThongTin);
void ChuanHoaThongTin(string &SinhVien);
void CapPhatBoNho(string *&luu_SinhVien, int dem_SV);
void TaiThongTin(SINHVIEN sv[], string html, int dem, int *ThongTin);

// Chuong trinh chinh
void main()
{
	// Mo File Template
	ifstream f;
	f.open("profile.html");
	if (!f)
	{
		cout << " Mo File That bai \n";
	}
	string html, line;
	while (!f.eof())
	{
		getline(f, line);
		html += line;
		html += "\n";
	}
	// Doc File .CSV
	ifstream g("students.csv");
	if (!g)
	{
		cout << " Mo File That bai \n";
	}
	SINHVIEN *sv;
	int tong_SV;
	cout << " Nhap so luong sinh vien: ";
	cin >> tong_SV;
	fflush(stdin);
	sv = new SINHVIEN[tong_SV];
	string *SinhVien = new string[tong_SV];
	string *luu_SinhVien = NULL;
	int dem_SV = 0;
	int *ThongTin = new int[tong_SV];
	for (int i = 0; i < tong_SV; i++)
	{
		getline(g, SinhVien[i]);
		XuLyThongTin(SinhVien[i],sv,i,luu_SinhVien, dem_SV,ThongTin);
	}
	
	// Dua vao file .html
	for (int i = 0; i < tong_SV; i++)
	{
		TaiThongTin(sv, html, i, ThongTin);
	}
	
	// Dong File
	f.close();
	g.close();
	
	// Giai phong bo nho da cap phat
	delete[] sv;
	delete[] SinhVien;
	delete[] luu_SinhVien;
	delete[] ThongTin;
}

// Ham Xu ly thong tin Sinh vien tu File .CSV
void XuLyThongTin(string SinhVien, SINHVIEN sv[], int dem, string *&luu_SinhVien, int dem_SV, int *&ThongTin)
{
	ChuanHoaThongTin(SinhVien);

	// Chuyen string thanh char*
	char *c = (char*)SinhVien.c_str();
	char *nl = NULL;
	char temp[] = "\"";

	// Cap phat vung nho cho luu_SinhVien de them thong tin cho sinh vien dau tien
	luu_SinhVien = new string[1];
	ThongTin[dem] = 0;

	// Xu ly du lieu tu File
	char *s = strtok_s(c, temp, &nl);
	while (s != NULL)
	{
		int n = strlen(s);
		for (int i = 0; i < n; i++)
		{
			luu_SinhVien[dem_SV].push_back(s[i]);
		}
		dem_SV++;
		CapPhatBoNho(luu_SinhVien, dem_SV);
		s = strtok_s(NULL, temp, &nl);
		ThongTin[dem] += 1;
	}

	// Dua du lieu vao struct ( tru so thich)
	sv[dem].mssv = luu_SinhVien[0];
	sv[dem].ten = luu_SinhVien[1];
	sv[dem].khoa = luu_SinhVien[2];
	sv[dem].nienkhoa = luu_SinhVien[3];
	sv[dem].ngaysinh = luu_SinhVien[4];
	sv[dem].hinhanh = luu_SinhVien[5];
	sv[dem].mota = luu_SinhVien[6];

	// Dua so thich vao struct
	sv[dem].sothich = new string[dem_SV - 6];
	for (int i = 0; i < dem_SV - 6; i++)
	{
		sv[dem].sothich[i] = luu_SinhVien[i + 7];
	}
}


// Ham Chuan hoa thong tin da doc duoc tu file .CSV
// VD: "1212123", "Lê Thị Thiên Trang"
// --->"1212123","Lê Thị Thiên Trang"
void ChuanHoaThongTin(string &SinhVien)
{
	for (int i = 0; i < SinhVien.length() - 1; i++)
	{
		if (SinhVien[i] == ',' && SinhVien[i + 1] == ' ')
		{
			SinhVien.erase(i, 3);
		}
	}
}

// Cap phat bo nho
void CapPhatBoNho(string *&luu_SinhVien, int dem_SV)
{
	string *temp = new string[dem_SV];
	for (int i = 0; i < dem_SV; i++)
	{
		temp[i] = luu_SinhVien[i];
	}
	luu_SinhVien = new string[dem_SV + 1];
	for (int i = 0; i < dem_SV; i++)
	{
		luu_SinhVien[i] = temp[i];
	}
}

// Ham tai cac thong tin vao file .html
void TaiThongTin(SINHVIEN sv[], string html, int dem, int* ThongTin)
{
	// Mo File
	string TenFile;
	TenFile = "Files/" + sv[dem].mssv + ".html";
	ofstream f(TenFile);

	// Dua thong tin vao file
	// Ma so sinh vien
	int n = 0;
	char *temp;
	do
	{
		temp = (char*)sv[dem].mssv.c_str();
		n = html.find("$mssv", n + 1, 5);
		if (n != std::string::npos)
		{
			html.replace(n, 5, temp);
		}
	} while (n != std::string::npos);

	// Ten
	n = 0;
	do
	{
		temp = (char*)sv[dem].ten.c_str();
		n = html.find("$ten", n + 1, 4);
		if (n != std::string::npos)
		{
			html.replace(n, 4, temp);
		}
	} while (n != std::string::npos);

	// Khoa
	n = 0;
	do
	{
		temp = (char*)sv[dem].khoa.c_str();
		n = html.find("$khoa", n + 1, 5);
		if (n != std::string::npos)
		{
			html.replace(n, 5, temp);
		}
	} while (n != std::string::npos);

	// Nien khoa
	n = 0;

	temp = (char*)sv[dem].nienkhoa.c_str();
	n = html.find("$nienkhoa");
	html.replace(n, 9, temp);

	// Ngay sinh
	n = 0;

	temp = (char*)sv[dem].ngaysinh.c_str();
	n = html.find("$ngaysinh");
	html.replace(n, 9, temp);


	// Hinh anh
	n = 0;

	temp = (char*)sv[dem].hinhanh.c_str();
	n = html.find("$hinhanh");
	html.replace(n, 8, temp);


	// Mo ta
	n = 0;

	temp = (char*)sv[dem].mota.c_str();
	n = html.find("$mota");
	html.replace(n, 5, temp);


	// So thich
	n = 0;
	int dem_SoThich = ThongTin[dem] - 7;

	n = html.find("$sothich");
	string sothich = "";
	for (int i = 0; i < dem_SoThich; i++)
	{
		sothich += "<li>" + sv[dem].sothich[i] + "</li>" + "\n";
	}

	temp = (char*)sothich.c_str();
	html.replace(n, 8, temp);



	// Dua du lieu vao file .html
	f << html;
	f.close();
}
