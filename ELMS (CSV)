from tkinter import *
from tkinter import messagebox
from tkinter import ttk
from tkinter import filedialog
import csv
import pandas as pd
import datetime
from datetime import timedelta
import calendar
import numpy as np
import matplotlib.pyplot as plt
import os

root = Tk()
root.title("Leave Management System")
root.minsize(1080,640)
root.geometry("1560x700")

emp_data_col = ["ID","NAME","DESIGNATION","EMPLOYEE TYPE",
		"DATE OF JOINING", "QUALIFICATIONS",
		"EXPERIENCE","SALARY","DATE OF LAST INCREMENT",
		"AADHAAR NO.","PAN CARD NO.","DATE OF EXIT"]
leave_entry_col = ["ID","NAME","DESIGNATION","EMPLOYEE TYPE",
		"Date Of Application", "Leave From", "Leave To", 
		"Leaves Entitled", "No. of Days", "Leaves Availed",
		"Leave Balance","REASON OF LEAVE"]
data_col = ['NAME','Apr2020','May2020','Jun2020','Jul2020','Aug2020','Sep2020',
			'Oct2020','Nov2020','Dec2020','Jan2021','Feb2021','Mar2021']
try:
	df_emp = pd.read_csv('empmanage.csv')
except:	
	df_emp = pd.DataFrame(columns=emp_data_col)
	df_emp.to_csv('empmanage.csv',index=False)
try:
	df_leave = pd.read_csv('leaves.csv')
except:
	df_leave = pd.DataFrame(columns=leave_entry_col)
	df_leave.to_csv('leaves.csv',index=False)
try:
	df_data = pd.read_csv('2020-21.csv')
except:
	df_data = pd.DataFrame(columns=data_col)
	df_data.to_csv('2020-21.csv',index=False)

leave_entitlement = 15
Entry_table =None

def frame_control(frame):
	global Entry_table
	if frame == "login":
		Home_Page.lift()
	if frame == "leave_man":
		Leave_management_page.lift()
	if frame == "entry_tch":
		Entry_page.lift()
		Entry_table = "Teaching"
	if frame == "entry_nontch":
		Entry_page.lift()
		Entry_table = "Non-Teaching"
	if frame == "entry_clsiv":
		Entry_page.lift()
		Entry_table = "CLASS IV STAFF"
	if frame == "emp_man":
		Emp_management_page.lift()

#FUNCTIONS FOR EMPLOYEE MANAGEMENT PAGE
def add_emp():
	Emp_entry_page.lift()
	global Emp_Manage_Option
	Emp_Manage_Option = "ADD"
	enable_fields()

def edit_emp():
	Emp_entry_page.lift()
	global Emp_Manage_Option
	Emp_Manage_Option = "EDIT"
	disable_fields()
	
def remove_emp():
	Emp_entry_page.lift()
	global Emp_Manage_Option
	Emp_Manage_Option = "DEL"
	disable_fields()

#DEFINING GENERAL PURPOSE FUNCTIONS OF PROGRAM

#FUCTION OF SUBMIT BUTTON
def submit(parameter):

	#FOR EMPLOYEE MANAGEMENT
	global Emp_Manage_Option
	if parameter == "emp_man":

		df_emp = pd.read_csv('empmanage.csv')
		df_data = pd.read_csv('2020-21.csv')
		all_id = df_emp['ID'].tolist()

		#GETTING THE VALUES FROM TEXTBOXES
		emp_type_entry['state']=NORMAL
		emp_id = int(emp_id_man_entry.get())
		doj = datetime.datetime.strptime(f'{doj_year_var.get()}-{doj_month_var.get()}-{doj_day_var.get()}','%Y-%m-%d')
		doli = f'{doli_year_var.get()}-{doli_month_var.get()}-{doli_day_var.get()}'
		doe = f'{doe_year_var.get()}-{doe_month_var.get()}-{doe_day_var.get()}'
		emp_name = str(add_emp_name_entry.get())
		emp_des = str(add_designation_entry.get())
		emp_type = str(emp_type_entry_var.get())
		qual = str(qual_entry.get())
		exp = int(exp_entry.get())
		salary = int(salary_entry.get())
		aadhaar = str(aadhaar_entry.get())
		pan_card = str(pan_card_entry.get())
		emp_type_entry['state']=DISABLED

		if Emp_Manage_Option == "ADD":

			try:
				if doli == '--':
					doli = np.nan
				else:
					doli = datetime.datetime.strptime(f'{doli_year_var.get()}-{doli_month_var.get()}-{doli_day_var.get()}','%Y-%m-%d')
				if doe == '--':
					doe = np.nan
				else:
					doe = datetime.datetime.strptime(f'{doe_year_var.get()}-{doe_month_var.get()}-{doe_day_var.get()}','%Y-%m-%d')
			
				try:
					if emp_id in all_id:
						messagebox.showwarning("WARNING", "Employee ID Not Unique!")
					else:
						df_row = pd.DataFrame([[emp_id,emp_name,emp_des,emp_type,doj,qual,int(exp),int(salary),doli,str(aadhaar),pan_card,doe]],columns=emp_data_col)
						df_emp = pd.concat([df_emp,df_row])
						df_emp.reset_index(drop=True, inplace=True)						
						df_row_data = pd.DataFrame(0,index=df_data.index, columns=data_col)
						df_row_data['NAME']=emp_name
						df_row_data.reset_index(drop=True, inplace=True)
						df_data = pd.concat([df_data,df_row_data])
						df_data.reset_index(drop=True, inplace=True)
						df_data.drop_duplicates(keep='first',inplace=True)
						df_data.to_csv('2020-21.csv',columns=data_col,index=False)
						df_emp.to_csv('empmanage.csv',columns=emp_data_col,index=False)
						clear("emp_man")
			
				except:
					messagebox.showwarning("WARNING", "Could Not Add Details!")
					clear("emp_man")
			except:
				messagebox.showerror('ERROR','Invalid Details Entered!')

		#SUBMIT FUNCTION FOR EDITING DETAILS OF AN EMPLOYEE
		elif Emp_Manage_Option == "EDIT":
			yesno = messagebox.askyesno('CONFIRM', 'Save Changes ?')
			if yesno:
				try:
					#CHECKING IF DATE OF LAST INCREMENT AND DATE OF EXIT HAVE BEEN ENTERED OR NOT
					if doli == '--':
						doli = np.nan
					else:
						doli = datetime.datetime.strptime(f'{doli_year_var.get()}-{doli_month_var.get()}-{doli_day_var.get()}','%Y-%m-%d')
					if doe == '--':
						doe = np.nan
					else:
						doe = datetime.datetime.strptime(f'{doe_year_var.get()}-{doe_month_var.get()}-{doe_day_var.get()}','%Y-%m-%d')				
					df_emp.loc[df_emp['ID']==emp_id,emp_data_col] = [emp_id,emp_name,emp_des,emp_type,doj,qual,int(exp),int(salary),doli,str(aadhaar),pan_card,doe]
					df_emp.to_csv('empmanage.csv',columns=emp_data_col,index=False)
					clear("emp_man")
				except:
					messagebox.showwarning('WARNING',"Could Not Save Changes!")
			else:
				clear("emp_man")

		#DELETING RECORD FOR AN EMPLOYEE
		elif Emp_Manage_Option == "DEL":
			yesno = messagebox.askyesno('CONFIRM', 'Are You Sure ?\nRecord shall be permanently deleted!')
			if yesno:
				try:					
					df_emp.set_index('ID')
					df_emp.drop(df_emp[df_emp['ID'] == emp_id].index,inplace=True)
					df_emp.to_csv('empmanage.csv',columns=emp_data_col,index=False)
					enable_fields()
					clear("emp_man")
					disable_fields()
				except:
					enable_fields()
					messagebox.showwarning("WARNING", "Unknown Error Occured!")
					clear("emp_man")
					disable_fields()
			else:
				enable_fields()
				clear('emp_man')
				disable_fields()
		else:
			messagebox.showinfo("Info", "No Option Selected!")

		emp_type_entry['state']=DISABLED

	#ENTERING LEAVE DETAILS
	if parameter == "leave_entry":
		global df_leave
		global Entry_table

		df_leave = pd.read_csv('leaves.csv')

		def insert_row():
			global df_leave
			df_row = pd.DataFrame([[emp_id,emp_name,emp_designation,Entry_table,doa,leave_frm,leave_to,noofdays,leave_entitlement,leave_av,leave_bal,reason_leave]],columns=leave_entry_col)
			df_leave = pd.concat([df_leave,df_row])
			df_leave.reset_index(drop=True, inplace=True)
			df_leave.to_csv('leaves.csv',columns=leave_entry_col,index=False)
			df_monthly = lbm(leave_frm,leave_to,emp_name)

		#EXTRACTING VALUES
		emp_id = int(emp_id_entry.get())
		emp_name = str(emp_name_entry.get())
		emp_designation = str(emp_designation_entry.get())
		doa = f'{doa_year_var.get()}-{doa_month_var.get()}-{doa_day_var.get()}'
		leave_frm = f'{leave_frm_year_var.get()}-{leave_frm_month_var.get()}-{leave_frm_day_var.get()}'
		leave_to = f'{leave_to_year_var.get()}-{leave_to_month_var.get()}-{leave_to_day_var.get()}'
		reason_leave = reason_leave_entry.get()
		noofdays = int(f'{entered_nod.get()}')
		leave_av = int(l_av) + noofdays
		leave_bal = int(leave_entitlement) - int(leave_av)
		
		#CHECKING IF LEAVE BALANCE IS ZERO?
		if leave_bal <0:
			messagebox.showwarning('WARNING',"Insufficient Leave Balance!\nWill be treated as Leave Without Pay!")
		insert_row()
		clear("submit")

def lbm(start,end,emp_name):
	
	df_data = pd.read_csv('2020-21.csv')
	df = df_data[df_data['NAME']==f'{emp_name}']
	start = datetime.datetime.strptime(start,'%Y-%m-%d')
	end = datetime.datetime.strptime(end,'%Y-%m-%d')

	d = df.to_dict('list')
	k = [x for x in d.keys()]
	M_list = [0,4,5,6,7,8,9,10,11,12,1,2,3]

	if (end.month >=4 and start.month>=4) or (end.month<4 and start.month<4):
		months = [x for x in range(start.month,end.month+1)]
		check = False
		year = start.year
	else:
		months = [x%12 if x!=12 else x for x in range(start.month,end.month+13)]
		check = True
		year = end.year

	if not check:
		for i in months[1:len(months)-1]:
			K = k[(M_list.index(i))]
			d[K][0] += int(''.join(calendar.month(year,i)[-3:-1]))
	else:
		for i in months[1:len(months)-1]:
			K = k[(M_list.index(i))]
			print(d[K])
			d[K][0] += int(''.join(calendar.month(year,i)[-3:-1]))

	if abs(end.month-start.month)>=1:
		K = k[(M_list.index(start.month))]
		d[K][0] += int(''.join(calendar.month(start.year,int(M_list.index(start.month)))[-3:-1]))-start.day+1
		K = k[(M_list.index(end.month))]
		d[K][0] += end.day

	else:
		K = k[(M_list.index(start.month))]
		d[K][0] += end.day-start.day+1
	df = pd.DataFrame(d)
	df.set_index(['NAME'],inplace=True,drop=True)
	df_data.set_index(['NAME'],inplace=True)
	df_data.drop(index=[f'{emp_name}'],axis=0,inplace=True)
	df_data = pd.concat([df_data,df])
	os.remove('2020-21.csv')
	df_data.to_csv('2020-21.csv')
	

#EXTRACTING EMPLOYEE DETAILS AND AUTOFILLING SOME FIELDS
def autofill(parameter):

	if parameter == "emp_man":

		global df_emp
		global Emp_Manage_Option
		global emp_man_name_var
		global emp_man_des_var
		global emp_man_qual_var
		global emp_man_exp_var
		global emp_man_sal_var
		global emp_man_aad_var
		global emp_man_pan_var
		global emp_type_entry_var

		df_emp = pd.read_csv('empmanage.csv')
		entered_id = emp_id_man_entry.get()

		if Emp_Manage_Option == "EDIT":
			enable_fields()

		try:
			data = df_emp[df_emp['ID'].astype(str).str.contains(f'{entered_id}')]
			autofill_data = data.values.tolist()
			emp_man_name_var.set(autofill_data[0][1])
			emp_man_des_var.set(autofill_data[0][2])
			emp_type_entry_var.set(autofill_data[0][3])
			emp_man_qual_var.set(autofill_data[0][5])
			emp_man_exp_var.set(autofill_data[0][6])
			emp_man_sal_var.set(autofill_data[0][7])
			emp_man_aad_var.set(autofill_data[0][9]) 
			emp_man_pan_var.set(autofill_data[0][10])

			doj = str(autofill_data[0][4])
			doli = str(autofill_data[0][8])
			doe = str(autofill_data[0][11])
			
			doj = doj.split(' ')
			doj = doj[0].split('-')
			doli = doli.split(' ')
			doli = doli[0].split('-')
			doe = doe.split(' ')
			doe = doe[0].split('-')

			doj_year_var.set(doj[0])
			doj_month_var.set(doj[1])
			doj_day_var.set(doj[2])
			
			if doe == ['nan']:
				doe_year_var.set("")
				doe_month_var.set("")
				doe_day_var.set("")
			else:
				doe_year_var.set(doe[0])
				doe_month_var.set(doe[1])
				doe_day_var.set(doe[2])
			if doli == ['nan']:
				doli_year_var.set("")
				doli_month_var.set("")
				doli_day_var.set("")
			else:
				doli_year_var.set(doli[0])
				doli_month_var.set(doli[1])
				doli_day_var.set(doli[2])
		except:
			messagebox.showwarning("WARNING",'Invalid Details!')
			clear("emp_man")
			disable_fields()

	if parameter == "leave_entry":

		entered_id = emp_id_entry.get()
		global l_av
		global l_bal
		global nod
		global Entry_table

		def set_values():
			entered_name.set(name)
			entered_designation.set(designation)
			entered_lav.set("Leave Availed : " + str(l_av))
			entered_lbal.set("Leave Balance : " + str(l_bal))

		data = df_emp[df_emp['ID'].astype(str).str.contains(f'{entered_id}')]
		autofill_data = data.values.tolist()
		stored_type = autofill_data[0][3]

		if stored_type == Entry_table:
			try:
				l_frm = datetime.datetime.strptime(f'{leave_frm_year_var.get()}-{leave_frm_month_var.get()}-{leave_frm_day_var.get()}','%Y-%m-%d')
				l_to = datetime.datetime.strptime(f'{leave_to_year_var.get()}-{leave_to_month_var.get()}-{leave_to_day_var.get()}','%Y-%m-%d')
				nod = (l_to - l_frm).days + 1
				entered_nod.set(nod)
			except:
				messagebox.showwarning("Info", "Invalid Date Entered!")

			try:
				name = str(autofill_data[0][1])
				designation = autofill_data[0][2]
				Entry_table = autofill_data[0][3]
			except:
				name = None
				designation = None
				messagebox.showerror("ERROR", "No Record Found!")

			try:
				data = df_leave[df_leave['ID'].astype(str).str.contains(f'{entered_id}')]
				autofill_data = data.values.tolist()
				l_av = autofill_data[-1][9]
				l_bal = autofill_data[-1][10]

			except:
				l_av = 0
				l_bal = leave_entitlement

			set_values()
		else:
			messagebox.showerror('ERROR','Invaid Employee Type!')

def lbm(start,end,emp_name):
	
	df_data = pd.read_csv('2020-21.csv')
	df = df_data[df_data['NAME']==f'{emp_name}']
	start = datetime.datetime.strptime(start,'%Y-%m-%d')
	end = datetime.datetime.strptime(end,'%Y-%m-%d')

	d = df.to_dict('list')
	k = [x for x in d.keys()]
	M_list = [0,4,5,6,7,8,9,10,11,12,1,2,3]

	if (end.month >=4 and start.month>=4) or (end.month<4 and start.month<4):
		months = [x for x in range(start.month,end.month+1)]
		check = False
		year = start.year
	else:
		months = [x%12 if x!=12 else x for x in range(start.month,end.month+13)]
		check = True
		year = end.year

	if not check:
		for i in months[1:len(months)-1]:
			K = k[(M_list.index(i))]
			d[K][0] += int(''.join(calendar.month(year,i)[-3:-1]))
	else:
		for i in months[1:len(months)-1]:
			K = k[(M_list.index(i))]
			print(d[K])
			d[K][0] += int(''.join(calendar.month(year,i)[-3:-1]))

	if abs(end.month-start.month)>=1:
		K = k[(M_list.index(start.month))]
		d[K][0] += int(''.join(calendar.month(start.year,int(M_list.index(start.month)))[-3:-1]))-start.day+1
		K = k[(M_list.index(end.month))]
		d[K][0] += end.day

	else:
		K = k[(M_list.index(start.month))]
		d[K][0] += end.day-start.day+1
	df = pd.DataFrame(d)
	df.set_index(['NAME'],inplace=True,drop=True)
	df_data.set_index(['NAME'],inplace=True)
	df_data.drop(index=[f'{emp_name}'],axis=0,inplace=True)
	df_data = pd.concat([df_data,df])
	os.remove('2020-21.csv')
	df_data.to_csv('2020-21.csv')
	return df


#BACK BUTTON.. LOWERS CURRENT FRAME
def back(parameter):

	if parameter == "Emp_man":
		clear("emp_man")
		Emp_management_page.lower()

	if parameter == "Emp_man_display":
		Emp_Man_Display_Page.lower()

	if parameter == "emp_entry":
		clear("emp_man")
		Emp_entry_page.lower()

	if parameter == "Leave_man_page":
		Leave_management_page.lower()

	if parameter == "Entry_page":
		clear("submit")
		Entry_page.lower()

	if parameter == "Display_page":
		Display_Page.lower()

#CLEAR BUTTON.. RESETS THE VALUES IN ALL TEXTBOXES TO DEFAULT
def clear(parameter):

	if parameter == "emp_man":
		emp_type_entry['state']=NORMAL
		emp_type_entry.delete(0,END)
		emp_type_entry['state']=DISABLED
		emp_id_man_entry.delete(0,END)
		add_emp_name_entry.delete(0,END)
		add_designation_entry.delete(0,END)
		qual_entry.delete(0,END)
		exp_entry.delete(0,END)
		salary_entry.delete(0,END)
		aadhaar_entry.delete(0,END)
		pan_card_entry.delete(0,END)
		doj_year_var.set("2020")
		doli_year_var.set("")
		doe_year_var.set("")
		doj_month_var.set("1")
		doli_month_var.set("")
		doe_month_var.set("")
		doj_day_var.set("1")
		doli_day_var.set("")
		doe_day_var.set("")

	if parameter == "submit":
		emp_id_entry.delete(0,END)
		entered_name.set("")
		entered_designation.set("")
		entered_nod.set("")
		reason_leave_entry.delete(0,END)
		doa_year_var.set("2020")
		leave_frm_year_var.set("2020")
		leave_to_year_var.set("2020")
		doa_month_var.set("1")
		leave_frm_month_var.set("1")
		leave_to_month_var.set("1")
		doa_day_var.set("1")
		leave_frm_day_var.set("1")
		leave_to_day_var.set("1")
		entered_lav.set("Leave Availed")
		entered_lbal.set("Leave Balance")

#DISABLES ALL TEXTBOXES TO PREVENT ENTRING OF DATA
def disable_fields():			
	add_emp_name_entry["state"] = DISABLED
	add_designation_entry["state"] = DISABLED
	emp_type_btn.configure(state=DISABLED)
	qual_entry["state"] = DISABLED
	exp_entry["state"] = DISABLED
	salary_entry["state"] = DISABLED
	aadhaar_entry["state"] = DISABLED
	pan_card_entry["state"] = DISABLED

#ENABLES ALL ENTRYBOXES TO ALLOW ENTERING DATA
def enable_fields():
	emp_id_man_entry["state"] = NORMAL
	add_emp_name_entry["state"] = NORMAL
	add_designation_entry["state"] = NORMAL
	emp_type_btn.configure(state=NORMAL)
	qual_entry['state'] = NORMAL
	exp_entry['state'] = NORMAL
	salary_entry['state'] = NORMAL
	aadhaar_entry['state'] = NORMAL
	pan_card_entry['state'] = NORMAL

#SETS OPTIONMENU FOR EMPLOYEE TYPE TO EMPLOYEE TYPE INSTEAD OF SHOWING SELECTED VALUE
def callback(*args):
	emp_type_entry['state']=NORMAL
	emp_type_entry_var.set(emp_type_var.get())
	emp_type_var.set("Employee Type")
	emp_type_entry['state']=DISABLED

#DISPLAYING THE RECORDS IN A TABLE
def display(parameter):
	global df
	global treev

	#SETTING THE STYLE OF THE TABLE
	style = ttk.Style()
	style.theme_use('clam')
	style.configure("mystyle.Treeview", font=('Calibri', 14), rowheight=50)
	style.configure("mystyle.Treeview.Heading", font=('Calibri', 18,'bold'))

	#EMPLOYEE MANAGEMENT TABLE DISPLAY
	if parameter == "Emp_list":
		
		Emp_Man_Display_Page.lift()
		df = pd.read_csv('empmanage.csv')
		for i in ['DATE OF JOINING',"DATE OF LAST INCREMENT","DATE OF EXIT"]:
			if i != np.nan:
				df[i] = pd.to_datetime(df[i]).dt.date
		df["AADHAAR NO."] = df["AADHAAR NO."].fillna(0).astype(np.int64)

		#CREATING THE TREEVIEW WIDGET TO DISPLAY TABLE
		treev = ttk.Treeview(Emp_Man_Display_Page, columns=df.columns.values, selectmode='browse', style="mystyle.Treeview",show=['headings'])
		
		#SETTING THE SCROLLBARS
		vsb = Scrollbar(Emp_Man_Display_Page, orient="vertical", command=treev.yview)
		vsb.place(relx=0.981,rely=0.0,relheight=0.85)
		hsb = Scrollbar(Emp_Man_Display_Page, orient="horizontal", command=treev.xview)
		hsb.place(relx=0.005,rely=0.85,relwidth=0.97)
		
		treev.configure(xscrollcommand=hsb.set,yscrollcommand=vsb.set)
		treev.place(relx=0.005, rely=0.0, relheight=0.85,relwidth=0.975)

		treev['columns']=("0","1","2","3","4","5","6","7","8","9","10","11")
		treev.heading("0", text="ID")
		treev.heading("1", text="NAME")
		treev.heading("2", text="DESIGNATION")
		treev.heading("3", text="EMP_TYPE")
		treev.heading("4", text="DATE OF JOINING")
		treev.heading("5", text="QUALIFICATIONS")
		treev.heading("6", text="EXPERIENCE")
		treev.heading("7", text="SALARY")
		treev.heading("8", text="DATE OF LAST INCREMENT")
		treev.heading("9", text="AADHAAR NO.")
		treev.heading("10", text="PAN CARD NO.")
		treev.heading("11", text="DATE OF EXIT")

		treev.column("0", minwidth=50,width=80)
		treev.column("1", minwidth=100,width=200)
		treev.column("2", minwidth=100,width=200)
		treev.column("3", minwidth=100,width=200)
		treev.column("4", minwidth=100,width=200)
		treev.column("5", minwidth=100,width=275)
		treev.column("6", minwidth=100,width=200)
		treev.column("8", minwidth=100,width=275)
		treev.column("7", minwidth=100,width=200)
		treev.column("9", minwidth=100,width=200)
		treev.column("10", minwidth=100,width=200)
		treev.column("11", minwidth=100,width=200)

		Update_Table("emp_man")

	#LEAVE ENTRIES RECORD
	if parameter == "leaves_entered":

		Display_Page.lift()
		df = pd.read_csv('leaves.csv')

		#TREEVIEW WIDGET TO DISPLAY TABLE
		treev = ttk.Treeview(Display_Page, columns=df.columns.values, selectmode='browse', style="mystyle.Treeview",show=['headings'])
		
		#SETTING SCROLLBARS
		vsb = Scrollbar(Display_Page, orient="vertical", command=treev.yview)
		vsb.place(relx=0.981,rely=0.0,relheight=0.85)
		hsb = Scrollbar(Display_Page, orient="horizontal", command=treev.xview)
		hsb.place(relx=0.005,rely=0.85,relwidth=0.97)
		treev.configure(xscrollcommand=hsb.set,yscrollcommand=vsb.set)
		
		treev.place(relx=0.005, rely=0.0, relheight=0.85,relwidth=0.975)
		treev['columns']=("0","1","2","3","4","5","6","7","8","9","10")

		treev.heading("0", text="ID")
		treev.heading("1", text="NAME")
		treev.heading("2", text="DESIGNATION")
		treev.heading("3", text="EMP_TYPE")
		treev.heading("4", text="Date Of Application")
		treev.heading("5", text="Leave From")
		treev.heading("6", text="Leave To")
		treev.heading("8", text="Leaves Entitled")
		treev.heading("7", text="No. of Days")
		treev.heading("9", text="Leaves Availed")
		treev.heading("10", text="Leaves Balance")

		treev.column("0", minwidth=50,width=80)
		treev.column("1", minwidth=100,width=275)
		treev.column("2", minwidth=100,width=275)
		treev.column("3", minwidth=100,width=200)
		treev.column("4", minwidth=100,width=225)
		treev.column("5", minwidth=100,width=200)
		treev.column("6", minwidth=100,width=200)
		treev.column("8", minwidth=100,width=200)
		treev.column("7", minwidth=100,width=200)
		treev.column("9", minwidth=100,width=200)
		treev.column("10", minwidth=100,width=200)
		
		Update_Table("leave_details")

#SETTING THE VALUES IN THE TABLE AS PER REQUIREMENTS    			
def Update_Table(parameter):

	rowLabels = df.index.tolist()
	
	#EMPLOYEE MANAGEMENT REFRESH
	def refresh(parameter):		
		for each_rec in range(len(df)):
			treev.insert("", 'end', values=list(df.loc[each_rec]))
		if parameter == "emp_man":
			length = 12
		elif parameter == "leave_details":
			length = 11
		for i in range(length):
			treev.column(i,anchor="center")
	
	#SORTING DATA AND DISPLAYING 	
	def sorted_display(parameter):

		treev.delete(*treev.get_children())

		if parameter == "Descending":
			order_by = False
		else:
			order_by = True
		rowLabels = df.index.tolist()
		df.sort_values([sort_by],ascending=[order_by],inplace=True)
		for i in range(len(df)):
    			treev.insert('',i, text=rowLabels[i], values=df.iloc[i,:].tolist())

	if parameter == "emp_man":
		refresh("emp_man")

	elif parameter == "emp_man_sort":		
		sort_by = emp_man_sort_var.get()
		order_by = emp_man_order_var.get()
		sorted_display(order_by)

	elif parameter == "leave_details":
		refresh("leave_details")

	elif parameter == "leave_details_sort":
		sort_by = sort_var.get()
		order_by = sort_type_var.get()
		sorted_display(order_by)

def graph(page):
	top = Toplevel()
	top.geometry('400x400')
	top.title('Data Visualization')
	if page == 'empmanage':
		pie = Button(top,text='Pie Chart',command=lambda: emp_types('pie')).pack()
		bar = Button(top,text='Bar Chart',command=lambda: emp_types('bar')).pack()
	elif page == 'leaves':
		name = StringVar()
		name.set('')
		emp_name = Entry(top,textvariable=name).pack()
		bar = Button(top,text='Bar Chart',command=lambda: emp_leaves(f'{name.get()}','bar')).pack()
		pie = Button(top,text='Pie Chart',command=lambda: emp_leaves(f'{name.get()}','pie')).pack()
		line = Button(top,text='Line Chart',command=lambda: emp_leaves(f'{name.get()}','line')).pack()
		scatter = Button(top,text='Scatter Chart',command=lambda: emp_leaves(f'{name.get()}','scatter')).pack()

def emp_types(graph):
	df = pd.read_csv('empmanage.csv')
	values = df['EMPLOYEE TYPE'].value_counts().to_dict()
	x_label = []
	y_label = []
	for k,v in values.items():
		x_label.append(k)
		y_label.append(v)
	if graph == 'pie':
		pie('type',x_label,y_label)
	elif graph == 'bar':
		bar('type',x_label,y_label)

def emp_leaves(emp,graph):
	df = pd.read_csv('2020-21.csv')
	values = df[df['NAME']==emp].to_dict()
	x_label = []
	y_label = []
	for k in values.keys():
		x_label.append(k)
	k = values.keys()
	for v in values.values():
		k = list(v.keys())
		K=k[0]
		y_label.append(v[K])
	x_label = x_label[1:]
	y_label = y_label[1:]

	if graph == 'bar':
		bar(emp,x_label,y_label)
	elif graph == 'pie':
		pie(emp,x_label,y_label)
	elif graph == 'line':
		line(emp,x_label,y_label)
	elif graph == 'scatter':
		scatter(emp,x_label,y_label)

def pie(graph,x_label,y_label):
	if graph == 'type':
		plt.figure(figsize=(10,10))
		plt.pie(y_label, labels=x_label, autopct='%1.1f%%',
		        shadow=True, startangle=90)
		plt.axis('equal')
		plt.title('Employee Types',fontsize=20)
		plt.show()

	else:
		plt.figure(figsize=(10,10))
		plt.pie(y_label, labels=x_label, autopct='%1.1f%%',
		        shadow=True, startangle=90)
		plt.axis('equal')
		plt.title('Leave Details',fontsize=20)
		plt.show()

def bar(graph,x_label,y_label):
	if graph == 'type':
		plt.figure(figsize=(10,10))
		plt.bar(x_label, y_label, width=0.5, align='center')
		plt.title('Employee Types',fontsize=20)
		plt.xlabel('Type', fontsize =15) 
		plt.ylabel('Count', fontsize =15) 
		plt.show()

	else:
		plt.figure(figsize=(10,10))
		plt.bar(x_label, y_label, width=0.5, align='center')
		plt.title('Leave Details',fontsize=20)
		plt.xlabel('Name', fontsize =15)
		plt.ylabel('Number of Leaves', fontsize =15) 
		plt.show()

def line(graph,x_label,y_label):

	plt.figure(figsize=(10,10))
	plt.plot(x_label,y_label)
	plt.show()

def scatter(graph,x_label,y_label):

	plt.figure(figsize=(10,10))
	plt.scatter(x_label,y_label)
	plt.show()


#TITLE LABEL AND DEVELOPER LABEL
titleLabel = Label(root, text=("EMPLOYEE AND LEAVE MANAGEMENT SYSTEM"), pady=10, font=("Ariel Black",36,"bold"), 
	relief=GROOVE, bd=10, justify=CENTER, bg="#9cc6d2")
titleLabel.place(relwidth=1, relheight=0.25)

dev_Label = Label(root, text="developed by - Vanshaj Bhatnagar  ", font=("Ariel Black", 14, "italic"),
	pady=10, justify=LEFT, relief=RAISED , anchor=E, bg="#9cc6d2")
dev_Label.place(relx=0, rely=0.9, relwidth=1, relheight=0.1)

#HOME PAGE
Home_Page = Frame(root, bg="#c5d4e1")
Home_Page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Home_Page.lift()

#MAIN BUTTONS
leave_man_btn = Button(Home_Page, text="LEAVE\nMANAGEMENT", relief=RAISED, font=("Book Antiqua", 20, "bold"),justify=CENTER,
	fg="#2c1396" ,command=lambda: frame_control("leave_man"))
leave_man_btn.place(relwidth=0.3, relx=0.55, rely=0.4)

emp_man_btn = Button(Home_Page, text="EMPLOYEE\nMANAGEMENT", relief=RAISED, font=("Book Antiqua", 20, "bold"),justify=CENTER,
	fg="#2c1396" ,command=lambda: frame_control("emp_man"))
emp_man_btn.place(relwidth=0.3, relx=0.15, rely=0.4)

#EMPLOYEE MANAGEMENT WINDOW
Emp_management_page = Frame(root, bg="#c5d4e1")
Emp_management_page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Emp_management_page.lower()

#BUTTONS ON EMPLOYEE MANAGEMENT PAGE
add_emp_btn = Button(Emp_management_page, text="ADD EMPLOYEE", relief=RAISED, font=("Book Antiqua", 20, "bold"),fg="#2c1396",justify=CENTER, command=lambda: add_emp())
add_emp_btn.place(width=300, relx=0.38, rely=0.15)
edit_emp_btn = Button(Emp_management_page, text="EDIT DETAILS", relief=RAISED, font=("Book Antiqua", 20, "bold"),fg="#2c1396",justify=CENTER, command=lambda: edit_emp())
edit_emp_btn.place(width=300, relx=0.38, rely=0.35)
remove_emp_btn = Button(Emp_management_page, text="REMOVE RECORD", relief=RAISED, font=("Book Antiqua", 20, "bold"),fg="#2c1396",justify=CENTER , command=lambda: remove_emp())
remove_emp_btn.place(width=300, relx=0.38, rely=0.55)
back_btn_emp_man = Button(Emp_management_page, text="BACK", relief=RAISED, font=("Book Antiqua", 12), command=lambda: back("Emp_man"))
back_btn_emp_man.place(relx=0.05, rely=0.875, relwidth=0.14, relheight=0.1)
display_emp_man = Button(Emp_management_page, text="DISPLAY", relief=RAISED, font=("Book Antiqua", 20, "bold"),fg="#2c1396", command=lambda: display("Emp_list"))
display_emp_man.place(width=300, relx=0.38, rely=0.75)

#EMPLOYEE DETAILS ENTRY PAGE
Emp_entry_page = Frame(root, bg="#c5d4e1")
Emp_entry_page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Emp_entry_page.lower()

#BUTTONS ON EMPLOYEE DETAILS ENTRY PAGE
submit_btn_emp_entry = Button(Emp_entry_page, text="SUBMIT", relief=RAISED, font=("Book Antiqua", 12),justify=CENTER, command=lambda: submit("emp_man"))
submit_btn_emp_entry.place(relwidth=0.14, relheight=0.1 ,relx=0.81, rely=0.875)
back_btn_emp_entry = Button(Emp_entry_page, text="BACK", relief=RAISED, font=("Book Antiqua", 12), command=lambda: back("emp_entry"))
back_btn_emp_entry.place(relx=0.05, rely=0.875, relwidth=0.14, relheight=0.1)
search_emp_entry = Button(Emp_entry_page, text="SEARCH", relief=RAISED, font=("Book Antiqua", 12), command=lambda: autofill('emp_man'))
search_emp_entry.place(relwidth=0.14, relheight=0.1, relx=0.43, rely=0.875)

#LABELS AND ENTRYBOXES ON EMPLOYEE DETAILS ENTRY WINDOW
emp_type_var = StringVar()
emp_type_var.set("Employee Type")
emp_type_optns = ["Teaching","Non-Teaching","CLASS IV STAFF"]
emp_type_btn = OptionMenu(Emp_entry_page, emp_type_var, *emp_type_optns)
emp_type_btn.place(relx=0.02, rely=0.225, relwidth=0.15, relheight=0.1)
emp_type_btn.configure(state=DISABLED)
emp_type_var.trace('w', callback)
emp_type_entry_var = StringVar()
emp_type_entry = Entry(Emp_entry_page, textvariable=emp_type_entry_var, font=("Book Antiqua", 12), justify=CENTER,state=DISABLED)
emp_type_entry.place(relx=0.175, rely=0.225, relwidth=0.15, relheight=0.1)

emp_id_man_label = Label(Emp_entry_page, text="Employee ID : ", font=("Book Antiqua", 12), relief=GROOVE,justify=CENTER)
emp_id_man_label.place(relx=0.02, rely=0.375, relwidth=0.15, relheight=0.1)
emp_id_man_entry = Entry(Emp_entry_page, font=("Book Antiqua", 12), justify=CENTER, state=NORMAL)
emp_id_man_entry.place(relx=0.175, rely=0.375, relwidth=0.15, relheight=0.1)

emp_man_name_var = StringVar()
add_emp_name_label = Label(Emp_entry_page, text="Employee Name : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
add_emp_name_label.place(relx=0.02, rely=0.525, relwidth=0.15, relheight=0.1)
add_emp_name_entry = Entry(Emp_entry_page, textvariable=emp_man_name_var,font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
add_emp_name_entry.place(relx=0.175, rely=0.525, relwidth=0.15, relheight=0.1)

emp_man_des_var = StringVar()
add_designation_label = Label(Emp_entry_page, text="Designation : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
add_designation_label.place(relx=0.02, rely=0.675, relwidth=0.15, relheight=0.1)
add_designation_entry = Entry(Emp_entry_page,textvariable=emp_man_des_var, font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
add_designation_entry.place(relx=0.175, rely=0.675, relwidth=0.15, relheight=0.1)

doj_year_var = StringVar()
doj_year_var.set("2020")
doj_month_var = StringVar()
doj_month_var.set("1")
doj_day_var = StringVar()
doj_day_var.set("1")
doj_years = {year for year in range(2000,2031)}
doj_months = {month for month in range(1,13)}
doj_days = {day for day in range(1,32)}

doj_label = Label(Emp_entry_page, text="Date of Joining : ", font=("Book Antiqua", 12), justify=CENTER)
doj_label.place(relx=0.35, rely=0.225, relwidth=0.15, relheight=0.1)
doj_year_opt = OptionMenu(Emp_entry_page, doj_year_var, *doj_years)
doj_month_opt = OptionMenu(Emp_entry_page, doj_month_var, *doj_months)
doj_day_opt = OptionMenu(Emp_entry_page, doj_day_var, *doj_days)
doj_year_opt.place(relx=0.505, rely=0.225, relwidth=0.05, relheight=0.1)
doj_month_opt.place(relx=0.555, rely=0.225, relwidth=0.05, relheight=0.1)
doj_day_opt.place(relx=0.605, rely=0.225, relwidth=0.05, relheight=0.1)

emp_man_qual_var = StringVar()
qual_label = Label(Emp_entry_page, text="Qualification\n(Year of Completion) : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
qual_label.place(relx=0.35, rely=0.375, relwidth=0.15, relheight=0.1)
qual_entry = Entry(Emp_entry_page,textvariable=emp_man_qual_var, font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
qual_entry.place(relx=0.505, rely=0.375, relwidth=0.15, relheight=0.1)

emp_man_exp_var = StringVar()
exp_label = Label(Emp_entry_page, text="Experience\n(No. of Years) : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
exp_label.place(relx=0.35, rely=0.525, relwidth=0.15, relheight=0.1)
exp_entry = Entry(Emp_entry_page, textvariable=emp_man_exp_var,font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
exp_entry.place(relx=0.505, rely=0.525, relwidth=0.15, relheight=0.1)

emp_man_sal_var = StringVar()
salary_label = Label(Emp_entry_page, text="Current Salary : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
salary_label.place(relx=0.35, rely=0.675, relwidth=0.15, relheight=0.1)
salary_entry = Entry(Emp_entry_page, textvariable=emp_man_sal_var,font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
salary_entry.place(relx=0.505, rely=0.675, relwidth=0.15, relheight=0.1)

doli_year_var = StringVar()
doli_year_var.set("")
doli_month_var = StringVar()
doli_month_var.set("")
doli_day_var = StringVar()
doli_day_var.set("")
doli_years = {year for year in range(2000,2031)}
doli_months = {month for month in range(1,13)}
doli_days = {day for day in range(1,32)}

doli_label = Label(Emp_entry_page, text="Date of Last Increment : ", font=("Book Antiqua", 12), justify=CENTER)
doli_label.place(relx=0.675, rely=0.225, relwidth=0.15, relheight=0.1)
doli_year_opt = OptionMenu(Emp_entry_page, doli_year_var, *doli_years)
doli_month_opt = OptionMenu(Emp_entry_page, doli_month_var, *doli_months)
doli_day_opt = OptionMenu(Emp_entry_page, doli_day_var, *doli_days)
doli_year_opt.place(relx=0.83, rely=0.225, relwidth=0.05, relheight=0.1)
doli_month_opt.place(relx=0.88, rely=0.225, relwidth=0.05, relheight=0.1)
doli_day_opt.place(relx=0.93, rely=0.225, relwidth=0.05, relheight=0.1)

emp_man_aad_var = StringVar()
aadhaar_label = Label(Emp_entry_page, text="Aadhaar No. : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
aadhaar_label.place(relx=0.675, rely=0.375, relwidth=0.15, relheight=0.1)
aadhaar_entry = Entry(Emp_entry_page, textvariable=emp_man_aad_var,font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
aadhaar_entry.place(relx=0.83, rely=0.375, relwidth=0.15, relheight=0.1)

emp_man_pan_var = StringVar()
pan_card_label = Label(Emp_entry_page, text="PAN Card No. : ", font=("Book Antiqua", 12), relief=GROOVE, justify=CENTER)
pan_card_label.place(relx=0.675, rely=0.525, relwidth=0.15, relheight=0.1)
pan_card_entry = Entry(Emp_entry_page, textvariable=emp_man_pan_var,font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
pan_card_entry.place(relx=0.83, rely=0.525, relwidth=0.15, relheight=0.1)

doe_year_var = StringVar()
doe_year_var.set("")
doe_month_var = StringVar()
doe_month_var.set("")
doe_day_var = StringVar()
doe_day_var.set("")
doe_years = {year for year in range(2000,2031)}
doe_months = {month for month in range(1,13)}
doe_days = {day for day in range(1,32)}

doe_label = Label(Emp_entry_page, text="Date of Exit : ", font=("Book Antiqua", 12), justify=CENTER)
doe_label.place(relx=0.675, rely=0.675, relwidth=0.15, relheight=0.1)
doe_year_opt = OptionMenu(Emp_entry_page, doe_year_var, *doe_years)
doe_month_opt = OptionMenu(Emp_entry_page, doe_month_var, *doe_months)
doe_day_opt = OptionMenu(Emp_entry_page, doe_day_var, *doe_days)
doe_year_opt.place(relx=0.83, rely=0.675, relwidth=0.05, relheight=0.1)
doe_month_opt.place(relx=0.88, rely=0.675, relwidth=0.05, relheight=0.1)
doe_day_opt.place(relx=0.93, rely=0.675, relwidth=0.05, relheight=0.1)


#EMPLOYEE MANAGEMENT DISPLAY WINDOW
Emp_Man_Display_Page = Frame(root, bg="#c5d4e1")
Emp_Man_Display_Page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Emp_Man_Display_Page.lower()

back_btn_emp_man = Button(Emp_Man_Display_Page, text="BACK", relief=RAISED, font=("Book Antiqua",12), command=lambda: back("Emp_man_display"))
back_btn_emp_man.place(relx=0.1, rely=0.895, relwidth=0.1, relheight=0.1)

#SORTING OPTIONS
emp_man_sort_var = StringVar()
emp_man_sort_var.set("ID")
sorting_parameters = ["ID","NAME","DESIGNATION","EMPLOYEE TYPE","DATE OF JOINING",
			"EXPERIENCE","SALARY","DATE OF LAST INCREMENT","DATE OF EXIT"]
emp_man_sort_btn = OptionMenu(Emp_Man_Display_Page, emp_man_sort_var, *sorting_parameters)
emp_man_sort_btn.place(relx=0.4, rely=0.895, relwidth=0.1, relheight=0.1)
emp_man_order_var = StringVar()
emp_man_order_var.set("Sort By")
order_parameters = ["Ascending", "Descending"]
emp_man_order_btn = OptionMenu(Emp_Man_Display_Page, emp_man_order_var, *order_parameters)
emp_man_order_btn.place(relx=0.25, rely=0.895, relwidth=0.1, relheight=0.1)

#REFRESH BUTTON
emp_man_refresh_btn = Button(Emp_Man_Display_Page, text="REFRESH", font=("Book Antiqua", 12), state=NORMAL, command=lambda: Update_Table("emp_man_sort"))
emp_man_refresh_btn.place(relx=0.55, rely=0.895, relwidth=0.1, relheight=0.1)

emp_man_graphs_btn = Button(Emp_Man_Display_Page, text="Show Graphs", font=("Book Antiqua", 12), state=NORMAL, command=lambda: graph('empmanage'))
emp_man_graphs_btn.place(relx=0.75, rely=0.895, relwidth=0.1, relheight=0.1)

#LEAVE MANAGEMENT PAGE
Leave_management_page = Frame(root, bg="#c5d4e1")
Leave_management_page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Leave_management_page.lower()

#BUTTONS FOR LEAVE ENTRY
teaching_btn = Button(Leave_management_page, text="TEACHING", relief=RAISED, font=("Book Antiqua", 20, "bold"),justify=CENTER,
	fg="#2c1396" ,command=lambda: frame_control("entry_tch"))
teaching_btn.place(width=300, relx=0.38, rely=0.15)
non_teaching_btn = Button(Leave_management_page, text="NON-TEACHING", relief=RAISED, font=("Book Antiqua", 20, "bold"),justify=CENTER,
	fg="#2c1396" ,command=lambda: frame_control("entry_nontch"))
non_teaching_btn.place(width=300, relx=0.38, rely=0.35)
classiv_btn = Button(Leave_management_page, text="CLASS IV STAFF", relief=RAISED, font=("Book Antiqua", 20, "bold"),justify=CENTER,
	fg="#2c1396" ,command=lambda: frame_control("entry_clsiv"))
classiv_btn.place(width=300, relx=0.38, rely=0.55)
display_btn_leaves = Button(Leave_management_page, text="DISPLAY", relief=RAISED, font=("Book Antiqua", 20, "bold"),justify=CENTER,
	fg="#2c1396" , command=lambda: display("leaves_entered"))
display_btn_leaves.place(width=300, relx=0.38, rely=0.75)

back_btn_entry = Button(Leave_management_page, text="BACK", relief=RAISED, font=("Book Antiqua", 12), command=lambda: back("Leave_man_page"))
back_btn_entry.place(relx=0.05, rely=0.85, relwidth=0.1, relheight=0.1)

#LEAVE ENTRY PAGE
Entry_page = Frame(root, bg="#c5d4e1")
Entry_page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Entry_page.lower()

submit_btn_entry = Button(Entry_page, text="SUBMIT", relief=RAISED, font=("Book Antiqua", 12), command=lambda: submit("leave_entry"))
submit_btn_entry.place(relx=0.85, rely=0.85, relwidth=0.1, relheight=0.1)

autofill_btn = Button(Entry_page, text="AUTOFILL", font=("Book Antiqua", 12), state=NORMAL, command=lambda: autofill("leave_entry"))
autofill_btn.place(relx=0.6, rely=0.85, relwidth=0.2, relheight=0.1)

back_btn_entry = Button(Entry_page, text="BACK", relief=RAISED, font=("Book Antiqua", 12), command=lambda: back("Entry_page"))
back_btn_entry.place(relx=0.1, rely=0.895, relwidth=0.1, relheight=0.1)

#ENTRY PAGE LABELS AND TEXTBOXES
doa_year_var = StringVar()
doa_year_var.set("2020")
doa_month_var = StringVar()
doa_month_var.set("1")
doa_day_var = StringVar()
doa_day_var.set("1")
doa_years = {year for year in range(2000,2031)}
doa_months = {month for month in range(1,13)}
doa_days = {day for day in range(1,32)}

leave_frm_year_var = StringVar()
leave_frm_year_var.set("2020")
leave_frm_month_var = StringVar()
leave_frm_month_var.set("1")
leave_frm_day_var = StringVar()
leave_frm_day_var.set("1")
leave_frm_years = {year for year in range(2000,2031)}
leave_frm_months = {month for month in range(1,13)}
leave_frm_days = {day for day in range(1,32)}

leave_to_year_var = StringVar()
leave_to_year_var.set("2020")
leave_to_month_var = StringVar()
leave_to_month_var.set("1")
leave_to_day_var = StringVar()
leave_to_day_var.set("1")
leave_to_years = {year for year in range(2000,2031)}
leave_to_months = {month for month in range(1,13)}
leave_to_days = {day for day in range(1,32)}

emp_id_label = Label(Entry_page, text="Employee ID : ", font=("Book Antiqua", 12), justify=CENTER)
emp_id_label.place(relx=0.115, rely=0.2, relwidth=0.15, relheight=0.1)
emp_id_entry = Entry(Entry_page, font=("Book Antiqua", 12), justify=CENTER)
emp_id_entry.place(relx=0.28, rely=0.2, relwidth=0.2, relheight=0.1)

entered_name = StringVar()
emp_name_label = Label(Entry_page, text="Name : ", font=("Book Antiqua", 12), justify=CENTER)
emp_name_label.place(relx=0.115, rely=0.35, relwidth=0.15, relheight=0.1)
emp_name_entry = Entry(Entry_page, textvariable=entered_name, font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
emp_name_entry.place(relx=0.28, rely=0.35, relwidth=0.2, relheight=0.1)

entered_designation = StringVar()
emp_designation_label = Label(Entry_page, text="Designation : ", font=("Book Antiqua", 12), justify=CENTER)
emp_designation_label.place(relx=0.115, rely=0.5, relwidth=0.15, relheight=0.1)
emp_designation_entry = Entry(Entry_page,textvariable=entered_designation, font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
emp_designation_entry.place(relx=0.28, rely=0.5, relwidth=0.2, relheight=0.1)

emp_doa_label = Label(Entry_page, text="Date of Application : ", font=("Book Antiqua", 12), justify=CENTER)
emp_doa_label.place(relx=0.115, rely=0.65, relwidth=0.15, relheight=0.1)
doa_year_opt = OptionMenu(Entry_page, doa_year_var, *doa_years)
doa_month_opt = OptionMenu(Entry_page, doa_month_var, *doa_months)
doa_day_opt = OptionMenu(Entry_page, doa_day_var, *doa_days)
doa_year_opt.place(relx=0.28, rely=0.65, relwidth=0.06, relheight=0.1)
doa_month_opt.place(relx=0.35, rely=0.65, relwidth=0.06, relheight=0.1)
doa_day_opt.place(relx=0.42, rely=0.65, relwidth=0.06, relheight=0.1)

leave_frm_label = Label(Entry_page, text="Leave From : ", font=("Book Antiqua", 12), justify=CENTER)
leave_frm_label.place(relx=0.515, rely=0.2, relwidth=0.15, relheight=0.1)
leave_frm_year_opt = OptionMenu(Entry_page, leave_frm_year_var, *leave_frm_years)
leave_frm_month_opt = OptionMenu(Entry_page, leave_frm_month_var, *leave_frm_months)
leave_frm_day_opt = OptionMenu(Entry_page, leave_frm_day_var, *leave_frm_days)
leave_frm_year_opt.place(relx=0.68, rely=0.2, relwidth=0.06, relheight=0.1)
leave_frm_month_opt.place(relx=0.75, rely=0.2, relwidth=0.06, relheight=0.1)
leave_frm_day_opt.place(relx=0.82, rely=0.2, relwidth=0.06, relheight=0.1)

leave_to_label = Label(Entry_page, text="Leave To : ", font=("Book Antiqua", 12), justify=CENTER)
leave_to_label.place(relx=0.515, rely=0.35, relwidth=0.15, relheight=0.1)
leave_to_year_opt = OptionMenu(Entry_page, leave_to_year_var, *leave_to_years)
leave_to_month_opt = OptionMenu(Entry_page, leave_to_month_var, *leave_to_months)
leave_to_day_opt = OptionMenu(Entry_page, leave_to_day_var, *leave_to_days)
leave_to_year_opt.place(relx=0.68, rely=0.35, relwidth=0.06, relheight=0.1)
leave_to_month_opt.place(relx=0.75, rely=0.35, relwidth=0.06, relheight=0.1)
leave_to_day_opt.place(relx=0.82, rely=0.35, relwidth=0.06, relheight=0.1)

entered_nod = StringVar()
no_of_days_label = Label(Entry_page, text="No. Of Days : ", font=("Book Antiqua", 12), justify=CENTER)
no_of_days_label.place(relx=0.515, rely=0.5, relwidth=0.15, relheight=0.1)
no_of_days_entry = Entry(Entry_page, textvariable=entered_nod, font=("Book Antiqua", 12), justify=CENTER, state=DISABLED)
no_of_days_entry.place(relx=0.68, rely=0.5, relwidth=0.2, relheight=0.1)

reason_leave_label = Label(Entry_page, text="Reason Of Leave : ", font=("Book Antiqua", 12), justify=CENTER)
reason_leave_label.place(relx=0.515, rely=0.65, relwidth=0.15, relheight=0.1)
reason_leave_entry = Entry(Entry_page, font=("Book Antiqua", 12), justify=CENTER)
reason_leave_entry.place(relx=0.68, rely=0.65, relwidth=0.2, relheight=0.1)

leave_entitlement_label = Label(Entry_page, text="Leave Entitlement :   " + str(leave_entitlement), font=("Book Antiqua", 12), justify=LEFT)
leave_entitlement_label.place(relx=0.05, rely=0.05, relwidth=0.25, relheight=0.1)

entered_lav = StringVar()
entered_lav.set("Leave Availed")
leave_availed_label = Label(Entry_page, textvariable=entered_lav, font=("Book Antiqua", 12), justify=LEFT)
leave_availed_label.place(relx=0.375, rely=0.05, relwidth=0.25, relheight=0.1)

entered_lbal = StringVar()
entered_lbal.set("Leave Balance")
leave_balance_label = Label(Entry_page, textvariable=entered_lbal, font=("Book Antiqua", 12), justify=LEFT)
leave_balance_label.place(relx=0.7, rely=0.05, relwidth=0.25, relheight=0.1)

#DISPLAY LEAVE RECORDS
Display_Page = Frame(root, bg="#c5d4e1")
Display_Page.place(relx=0, rely=0.25, relheight=0.65, relwidth=1)
Display_Page.lower()

#SORTING OPTIONS
sort_var = StringVar()
sort_var.set("ID")
sorting_parameters_leaves = ["ID","NAME","DESIGNATION","EMPLOYEE TYPE","Date Of Application", "Leave From", "Leave To", "No. of Days", "Leaves Availed", "Leave Balance"]
sort_btn = OptionMenu(Display_Page, sort_var, *sorting_parameters_leaves)
sort_btn.place(relx=0.4, rely=0.895, relwidth=0.1, relheight=0.1)

sort_type_var = StringVar()
sort_type_var.set("Ascending")
sort_type_optns = ["Ascending","Descending"]
asc_desc_btn = OptionMenu(Display_Page, sort_type_var, *sort_type_optns)
asc_desc_btn.place(relx=0.25, rely=0.895, relwidth=0.1, relheight=0.1)

#REFRESH TABLE BUTTON
refresh_btn = Button(Display_Page, text="REFRESH", font=("Book Antiqua", 12), state=NORMAL, command=lambda: Update_Table("leave_details_sort"))
refresh_btn.place(relx=0.55, rely=0.895, relwidth=0.1, relheight=0.1)

leaves_graphs_btn = Button(Display_Page, text="Show Graphs", font=("Book Antiqua", 12), state=NORMAL, command=lambda: graph('leaves'))
leaves_graphs_btn.place(relx=0.75, rely=0.895, relwidth=0.1, relheight=0.1)

back_btn_display = Button(Display_Page, text="BACK", relief=RAISED, font=("Book Antiqua", 12), command=lambda: back("Display_page"))
back_btn_display.place(relx=0.1, rely=0.895, relwidth=0.1, relheight=0.1)

root.mainloop()
