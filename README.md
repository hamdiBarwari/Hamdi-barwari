#include <iostream>
#include <string>
using namespace std;

const int K = 2, D = 3, n = 15;

class Hospital {
private:
	string Name_Hospital, Place, History;
	int Num_Of_ambulance, Storey, Num_Of_Doctor_one_storey, Num_Of_Room_one_storey;
	int  Total_Employe, Total_sister, Total_Doctor;

public:
	int R, *Room;
	static int Total_Salary_Hospital;

	Hospital() {  }
	Hospital(string Name, string history, string Place, int Storey, int Num_Of_Doctor_one_storey, int Num_Of_Room_one_storey, int Num_Of_ambulance) {

		this->Name_Hospital = Name;
		this->History = history;
		this->Place = Place;
		this->Storey = Storey;
		this->Num_Of_Doctor_one_storey = Num_Of_Doctor_one_storey;
		this->Num_Of_Room_one_storey = Num_Of_Room_one_storey;
		this->Num_Of_ambulance = Num_Of_ambulance;
		//Room = Storey * Num_Of_Room_one_storey;
		Total_Doctor = Storey * Num_Of_Doctor_one_storey;
		Total_sister = Total_Doctor * 2;
		Total_Employe = Num_Of_Room_one_storey + Num_Of_Doctor_one_storey;

		R = Storey * Num_Of_Room_one_storey;//to get all Room num.
		Room = new int[R];
		for (int i = 0; i < R; i++)
		{
			Room[i] = i + 1;
		}
	}

	void Display_Hospital_Info() {
		cout << "\n\tName:-----------------> " << Name_Hospital << "<-----------------\n";
		cout << "\tHistory: " << History << endl;
		cout << "\tPlace: " << Place << endl;
		cout << "\tStorey: " << Storey << endl;
		cout << "\tRoom: " << R << endl;
		cout << "\t  Total_Doctor: " << Total_Doctor << endl;
		cout << "\t  Total_sister: " << Total_sister << endl;
		cout << "\t  Total_Employe: " << Total_Employe << endl;
	}

	int Set_Total_Doctor() { return Total_Doctor; }
	int Set_Total_Sister() { return Total_sister; }
	int Set_Total_Employe() { return Total_Employe; }

};//End Class Hospital


class Person :public Hospital {//inheretence because have Static total salary
protected:
	string  *Address, *Name, *Gender, *specializ;
	int *Age, *Number_Phone, *ID, *Salary, *Room_person;
public:
	Person() {}
	Person(string name, int Num) {
		Name = new string[Num];
		Number_Phone = new int[Num];
		ID = new int[Num];
		Gender = new string[Num];
		Address = new string[Num];
		Salary = new int[Num];
		Age = new int[Num];
		Room_person = new int[Num];
		specializ = new string[Num];
	}



	void Display(string name, int Number) {
		for (int i = 0; i < Number; i++)
		{
			cout << "\n\t Name " << i + 1 << " is: " << Name[i] << endl;
			cout << "\tID      " << Name[i] << " is: " << ID[i] << endl;
			cout << "\tAge     " << Name[i] << " is: " << Age[i] << endl;
			cout << "\tGender  " << Name[i] << " is: " << Gender[i] << endl;
			cout << "\tSalary  " << Name[i] << " is: " << Salary[i] << endl;
			cout << "\tAddress " << Name[i] << " is: " << Address[i] << endl;
			cout << "\tRoom    " << Name[i] << " is:" << Room_person[i] << endl;
			if (name == "Doctor")
				cout << "\tNumber of Phone " << Name[i] << " is: " << Number_Phone[i] << endl;

			else if (name == "Sister")
				cout << "\tIt is specializ : " << specializ[i] << endl;

			else
				cout << "-----Print Ending!!\n";
		}
	}

	~Person() {
		delete[]Name;
		delete[]Gender;
		delete[]Age;
		delete[]Number_Phone;
		delete[]ID;
		delete[]Salary;
		delete[]Room_person;
		delete[]Address;
	}
};

class Doctor :public Person {//error nahat ko ma hospital inhe. nakre chonko  ma d person da ya hay
private:

	int Num_specializ_Doctor;
	int Total_Salary_Doctor;
	//as nashem bihaya bi dema array di constructore da jbar hinde min dynamic bkarenna
public:

	Doctor() {}
	Doctor(Hospital H, string n1, int p1) :Person(n1, p1)//inheretance constructor
	{//i get a object because i dont can get total doctor in inheretamce 0
		//if i get friend har de objecte chekam lawa
		n1 = "Doctor";
		p1 = H.Set_Total_Doctor();
		cout << "\n-------------> Total Doctor is <-------------{ " << H.Set_Total_Doctor() << " }------------->" << endl;
		for (int i = 0; i < H.Set_Total_Doctor(); i++)
		{
			cout << "\n\tEnter Name of Doctor [" << i + 1 << "]: ";
			cin >> Name[i];
			cout << "\tAge is : ";
			cin >> Age[i];
			cout << "\tID is : ";
			cin >> ID[i];
			cout << "\tNumber of phone is : ";
			cin >> Number_Phone[i];
			cout << "\tGender must be male or female: ";
			cin >> Gender[i];
			while (Gender[i] != "male" && Gender[i] != "female") {
				cout << "\tpleas make soure you typet Gender corect: ";
				cin >> Gender[i];
			}//i can in any Condi...

			cout << "\tRoom bettween [1] To [" << H.R << "] : ";
			cin >> Room_person[i];
			while (Room_person[i] > H.R) {
				cout << "\tpleas make soure you typet Room corect: ";
				cin >> Room_person[i];
			}

			cout << "\t number of specialization Doctor " << Name[i] << " is : ";
			cin >> Num_specializ_Doctor;
			specializ = new string[Num_specializ_Doctor];
			for (int j = 0; j < Num_specializ_Doctor; j++)
			{
				cout << "\tEnter specialization " << j << " of " << Name[i] << ": ";
				cin >> specializ[j];
			}
			cout << "\tSalary " << Name[i] << " is: ";
			cin >> Salary[i];

			H.Total_Salary_Hospital = Total_Salary_Doctor = Total_Salary_Doctor + Salary[i];
		}
	}//end construcor

	void Print_Info_Doctor(Hospital H) {
		Display("Doctor", H.Set_Total_Doctor());//Function in Person
		for (int j = 0; j < Num_specializ_Doctor; j++) {
			cout << "\tIt is specializ " << j + 1 << " is: " << specializ[j] << endl;
		}
	}

	void Set_Total_Salaray_Doctor() { cout << "Total Salary is:" << Total_Salary_Doctor; }

	friend void Get_Doctor_ID(Hospital &, Doctor &);

};//end Class Doctor

void Get_Doctor_ID(Hospital &H, Doctor &D) {
	int id;
	cout << "\t\tEnter ID of Doctor Doyo want to Get info: ";	cin >> id;
	for (int i = 0; i < H.Set_Total_Doctor(); i++) {
		if (id == D.ID[i])
		{
			cout << "\n\t  **********************\n\t  Name Doctor is: " << D.Name[i] << endl;
			cout << "\tIt is ID is: " << D.ID[i] << endl;
			cout << "\tIt is Age is: " << D.Age[i] << endl;
			cout << "\tNumber of Phone is: " << D.Number_Phone[i] << endl;
			cout << "\tGender is: " << D.Gender[i] << endl;
			cout << "\tsalary is: " << D.Salary[i] << endl;
			cout << "\tRoom is: " << D.Room_person[i] << endl;
			for (int j = 0; j < D.Num_specializ_Doctor; j++)
				cout << "\tIt is specializ the " << D.Name[i] << " is: " << D.specializ[j] << endl;
		}
		else { cout << "\t  ID undefine!!\n"; }
	}
	cout << "\t  **********************\n\n";
}/////////////////////////////////////////////////////////////End Friend Function_Doctor




class Sister :public Person {
public:
	int Total_Salary_Sister;
public:
	Sister() {}
	Sister(Hospital H, string n2, int p2) :Person(n2, p2) {
		n2 = "Sister";
		p2 = H.Set_Total_Sister();
		cout << "\n\t  -------------> Total Sister is <-------------{ " << H.Set_Total_Sister() << " }------------->" << endl;
		for (int i = 0; i < H.Set_Total_Sister(); i++)
		{
			cout << "\n\t  Enter Name of Sister [" << i + 1 << "]: ";
			cin >> Name[i];
			cout << "\tID is : ";
			cin >> ID[i];
			cout << "\tAge is : ";
			cin >> Age[i];
			cout << "\tAddress is : ";
			cin >> Address[i];

			cout << "\tGender must be male or female: ";
			cin >> Gender[i];
			while (Gender[i] != "male" && Gender[i] != "female") {
				cout << "\tpleas make soure you typet Gender corect: ";
				cin >> Gender[i];
			}

			cout << "\tSalary is: ";
			cin >> Salary[i];
			cout << "\tspecializ is: ";
			cin >> specializ[i];

			cout << "\tRoom bettween [1] To [" << H.R << "] : ";
			cin >> Room_person[i];
			while (Room_person[i] > H.R) {
				cout << "\tpleas make soure you typet Room corect: ";
				cin >> Room_person[i];
			}

			Total_Salary_Sister = Total_Salary_Sister + Salary[i];
			H.Total_Salary_Hospital = Total_Salary_Sister;
		}
	}

	void Print_Info_Sister(Hospital H)
	{
		Display("Sister", H.Set_Total_Sister());//Function in Person
		cout << "\tIt is Total Salary the is: " << Total_Salary_Sister << endl;
	}

	void Set_Total_Salaray_Sister() { cout << "\tTotal Salary is:" << Total_Salary_Sister << endl; }
	friend void Get_Sister_ID(Hospital &, Sister &);
};

void Get_Sister_ID(Hospital &H, Sister &S) {
	int id;
	cout << "\t  Enter ID of Sister Doyo want to Get info: ";  cin >> id;
	for (int i = 0; i < H.Set_Total_Sister(); i++) {
		if (id == S.ID[i])
		{
			cout << "\n\t**********************\n\t  Name Sister is: " << S.Name[i] << endl;
			cout << "\tID Sister is: " << S.ID[i] << endl;
			cout << "\tAddress Sister is: " << S.Address[i] << endl;
			cout << "\tAge Sister is: " << S.Age[i] << endl;
			cout << "\tGender Sister is: " << S.Gender[i] << endl;
			cout << "\tSalary sister is: " << S.Salary[i] << endl;
			cout << "\tRoom is: " << S.Room_person[i] << endl;
		}
		cout << "\t**********************\n\n";
	}
}////////////////////////////////////////////////////End Friend Function_Sister




class Employee :public Person {//i can not inheretance to person beause name-age..same name doctor
public:
	int Total_Salary_Employe;

public:
	Employee() {}
	Employee(Hospital H, string n3, int p3) : Person(n3, p3)//inheretance constructor
	{
		n3 = "Employee";
		p3 = H.Set_Total_Employe();

		cout << "\n\t  -------------> Total Employe is <-------------{ " << H.Set_Total_Employe() << " }------------->" << endl;
		for (int i = 0; i < H.Set_Total_Employe(); i++)
		{
			cout << "\n\t  Enter Name of Employe [" << i + 1 << "]: ";
			cin >> Name[i];
			cout << "\tID is : ";
			cin >> ID[i];
			cout << "\tAge is : ";
			cin >> Age[i];

			cout << "\tGender must be male or female: ";
			cin >> Gender[i];
			while (Gender[i] != "male" && Gender[i] != "female") {
				cout << "\tpleas make soure you typet Gender corect: ";
				cin >> Gender[i];
			}

			cout << "\tWork Employee is: ";
			cin >> specializ[i];
			cout << "\tSalary is: ";
			cin >> Salary[i];
			cout << "\tAddress is: ";
			cin >> Address[i];

			cout << "\tRoom bettween [1] To [" << (H.R - 1) << "] : ";
			cin >> Room_person[i];
			while (Room_person[i] > H.R) {
				cout << "\tpleas make soure you typet Room corect: ";
				cin >> Room_person[i];
			}

			Total_Salary_Employe = Total_Salary_Employe + Salary[i];
			H.Total_Salary_Hospital = Total_Salary_Employe;
		}
	}//End Constructor

	void Print_Info_Employe(Hospital H) {
		Display("Employee", H.Set_Total_Employe());//Function in Person
		cout << "\tIt is Total Salary the is: " << Total_Salary_Employe << "\n\n";
	}

	void Set_Total_Salaray_Employe() { cout << "Total Salary is:" << Total_Salary_Employe << endl; }
	friend void Get_Employe_ID(Hospital &, Employee &);

};//end class employee

void Get_Employe_ID(Hospital &H, Employee &E) {
	int id;
	cout << "\tEnter ID of Employe Doyo want to Get info: ";  cin >> id;
	for (int i = 0; i < H.Set_Total_Employe(); i++)
	{
		if (id == E.ID[i])
		{
			cout << "\n\t  **********************\n\t  Name Employe is: " << E.Name[i] << endl;
			cout << "\tID Employe is: " << E.ID[i] << endl;
			cout << "\tAge Employe is: " << E.Age[i] << endl;
			cout << "\tGender Employe is: " << E.Gender[i] << endl;
			cout << "\tSalary Employe is: " << E.Salary[i] << endl;
			cout << "\tWork Employe is: " << E.specializ[i] << endl;
			cout << "\tRoom is: " << E.Room_person[i] << endl;
		}
		cout << "\t  **********************\n\n";
	}
}///////////////////////////////////////////////////////////End Feiend_Function_Employe

class Patient {
private:
	string Name[n], Gender[n], Sick[n], Blood[n];
	int Age[n], Number_Phone[n];
	static int Front, Rear;
	// n is global scope
public:
	void insert(string Name, int Age, int Number_Phone, string Gender, string Sick, string Blood) {
		if (Full())
			cout << "\tQueue is Full\n";

		else {
			Rear++;
			this->Name[Rear] = Name;
			this->Sick[Rear] = Sick;
			this->Blood[Rear] = Blood;
			this->Age[Rear] = Age;
			this->Number_Phone[Rear] = Number_Phone;
			while (Gender != "male" && Gender != "female") {
				cout << "\tpleas make soure you typet Gender corect: ";
				cin >> Gender;
				this->Gender[Rear] = Gender;
			}
		}
	}

	int Full() {
		if (Rear == n)	return 1;
		else		return 0;
	}

	void Remov() {
		string P, G, S, B;
		int Pho, A;
		if (Empty())
			cout << "\tQueue is Empty";
		else {
			Front++;
			P = Name[Front];
			S = Sick[Front];
			B = Blood[Front];
			G = Gender[Front];
			Pho = Number_Phone[Front];
			A = Age[Front];
		}
	}

	int Empty() {
		if (Rear == Front)	return 1;
		else	return 0;
	}

	void Print() {
		if (Empty())
			cout << "\tQueue is Empty \n";
		else
			for (int i = Front + 1; i <= Rear; i++)
				cout << Name[i] << "  " << Age[i] << "  " << Blood[i] << "  " << Sick[i] << "  " << Number_Phone[i] << "  " << Gender[i] << "\t\t";
	}

	int Start() {
		Patient People;
		int Choice, Total, age, number_phone;
		string name, gender, sick, blood;
		cout << "\n\t   *********************\n";
		cout << "\t   1 - insert ifo The People in the Queue \n";
		cout << "\t   2- remov People Info in the Queue \n";
		cout << "\t   3- Print ifo The People in the Queue \n";
		cout << "\t   4- Exit Queue Patient \n\t   *********************\n";
		cout << "\n\t    Enter the Choice: ";
		cin >> Choice;
		while (Choice != 4)
		{
			if (Choice == 1)
			{
				cout << "\tHow many paient do you want to insert queue: ";
				cin >> Total;
				for (int i = 1; i <= Total; i++)
				{
					cout << "\t  Inserting the Patient " << i << " name:";
					cin >> name;
					cout << "\tInserting the Blood it :";
					cin >> blood;
					cout << "\tInserting the Age it :";
					cin >> age;
					cout << "\tInserting the Sick of it :";
					cin >> sick;
					cout << "\tInserting the Number of Phone :";
					cin >> number_phone;

					cout << "\tInserting the Gender Maile or Femail :";
					cin >> gender;
					while (gender != "male" && gender != "female") {
						cout << "\tpleas make soure you typet Gender corect: ";
						cin >> gender;
					}

					People.insert(name, age, number_phone, gender, sick, blood);
					cout << endl;
				}
			}
			if (Choice == 2)
			{
				cout << "How many paient doyo wand to Remov queue: ";
				cin >> Total;
				for (int i = 1; i <= Total; i++)
					People.Remov();
			}
			if (Choice == 3)
				People.Print();

			cout << "\n\tin Queue Enter another choice: ";
			cin >> Choice;
		}//end While
		return 0;
	}//end Statr Function
};//////////////////////////////////////////////////////////////////end class Queue



class pharmacy :public Hospital {
protected:
	string medicament[D];
	double price[D];
	string PRO[D];
	string EXP[D];
public:
	pharmacy() {
		cout << "\n\t  enter info pharmsy\n";
		for (int i = 0; i < D; i++)
		{
			cout << "\tenter medicament " << i + 1 << ": ";
			cin >> medicament[i];
			cout << "\tenter Price:  ";
			cin >> price[i];
			cout << "\tenter Product:  ";
			cin >> PRO[i];
			cout << "\tenter Expire:  ";
			cin >> EXP[i];
		}
	}

	void Set_Name()
	{
		string k;
		for (int i = 0; i < D; i++) {
			cout << " \tinspect Kind " << i << " : " << medicament[i] << endl;
		}
		cout << "\t  Enter the KInd info do you want learn about: ";
		cin >> k;
		for (int i = 0; i < D; i++) {
			if (medicament[i] == k) {
				cout << "\tmedicament is: " << medicament[i] << "\t Price: " << price[i];
				cout << "\t Time Product: " << PRO[i] << "\t Time Expire: " << EXP[i] << endl;
			}
		}
	}

};////////////////////////////////////////end class Pharmacy

class inspect {
private:
	string kind[K];
	double time[K];
	double price[K];
public:
	inspect()
	{
		for (int i = 0; i < K; i++) {
			cout << "\tEnter inspect Kind [ " << i + 1 << " ]: ";
			cin >> kind[i];
			cout << "\tEnter inspect time:";
			cin >> time[i];
			cout << "\tEnter inspect price:";
			cin >> price[i];
		}
	}
	void set_Name()
	{
		string k;
		cout << "\tEnter the KInd info do you want learn about: \n";
		for (int i = 0; i < K; i++) {
			cout << "\tinspect Kind " << i << " : " << kind[i] << endl;
		}
		cin >> k;
		for (int i = 0; i < K; i++) {
			if (kind[i] == k)
				cout << "\tKind is: " << kind[i] << "\t Time: " << time[i] << "\t Price: " << price[i] << endl;
		}
	}
};////////////////////////////////////////End inspect class

class Stor {
private:
	int blood_device, pumped_device, sugar_device;
	int C = 0;
	static int count;

public:
	Stor() {}
	void stor_inspect(int x = 0, int y = 0, int z = 0)
	{
		blood_device = x;
		pumped_device = y;
		sugar_device = z;

		C = C + (blood_device + pumped_device + sugar_device);
		Place();
		cout << "\tAll Device " << C << endl;
	}

	void Stor_Pharmacy() {
		int c = 0;
		const int SP = 2;//all medicamnet in stor
		string medicament[SP], EXP[SP], PRO[SP];

		cout << "\tTotal count medicament in Store is {" << SP << "}\n";
		for (int i = 0; i < SP; i++)
		{
			cout << "\tenter medicament [" << i << "]: ";
			cin >> medicament[i];
			cout << "\tenter Product:  ";
			cin >> PRO[i];
			cout << "\tenter Expire:  ";
			cin >> EXP[i];
		}

		for (int i = 0; i < SP; i++)
		{
			if (EXP[i] < PRO[i])
				c++;
		}

		cout << "\ttotal expire is: " << c << endl;
		C = C + SP;
		Place();
	}

	void Place() {
		if (C == count || C > count)    cout << "\tStore is full!!!\n";
		else if (C < count)
			cout << "\tcan have empty Place " << count - C << endl;
	}

	static void Get_Total_Mony_Hospital(Hospital H) {
		cout << "\t>>>>>>>" << H.Total_Salary_Hospital << endl;
	}

	void Represent() {
		cout << "\n\tOverloading :\n";
		int c;
		cout << "\t  1- Empty\n";
		cout << "\t  2- Add one item\n";
		cout << "\t  3- Add two item\n";
		cout << "\t  4- Add three item\n";
		cout << "\t  enter your choice: ";
		cin >> c;

		if (c == 1)
			stor_inspect();
		else if (c == 2)
		{
			int x;
			cout << "\tEnter number in blood device:"; cin >> x;
			stor_inspect(x);
		}
		else if (c == 3)
		{
			int x, y;
			cout << "\tEnter blood device&zxt device :";
			cin >> x >> y;
			stor_inspect(x, y);
		}
		else if (c == 4)
		{
			int x, y, z;
			cout << "\tEnter blood device&zxt device&sugar_device:";
			cin >> x >> y >> z;
			stor_inspect(x, y, z);
		}
		else cout << "\tNot overloaded\n";
	}
};

struct car
{
public:
	string driver;
	int noofcar;
};

const int C = 4;
class park
{
private:

	static int no_car;
	car c[C];
public:
	int parkk(int n)
	{
		if (no_car <= C)
		{
			for (int i = 0; i < n; i++)
			{
				no_car++;
				cout << "Enter the name of  the  car driver  " << i + 1 << endl;
				cin >> c[i].driver;
				c[i].noofcar = rand();
			}
			cout << "Total car in park: " << no_car << endl;
			bool l;
			cout << "remove  car in park enter 1 and 0 to exit this program :" << endl;
			cin >> l;
			if (l == 0) {
				return 0;
			}
			else
				start_park();
		}
		else {
			cout << "park is full!!\n";
			bool l;
			cout << "if you want remove  car in park you have only " << no_car << "enter 1" << endl;
			cin >> l;
			if (l)
				start_park();
			else return 0;
		}
	}
	int k;
	int start_park() {


		cout << "Enter number of car to remove the park Btween 0 and " << no_car << endl;
		cin >> k;
		while (k<0 || k>no_car) {
			cout << "set Correct :";
			cin >> k;
		}
		int p = no_car;
		p = p - k;
		cout << "Total car in park after remove: " << p << endl;
		cout << "if you want add new car in park you have only " << p << " car in park total place is " << no_car << "\n";
		cout << "you can add the car \n enter number incorect to exit\n";
		int h;
		cin >> h;
		if (h > 0 && h <= no_car)
		{
			parkk(h);
		}
		else
		{
			return 0;
		}
	}
	void Print_car_park() {
		for (int i = 0; i < no_car - k; i++)
		{
			cout << "Driver " << i + 1 << " :" << c[i].driver << endl;
			cout << "number of car :" << c[i].noofcar << endl;
		}
	}
};

int park::no_car = 0;
int Patient::Rear = 0;
int Patient::Front = 0;
int Employee::Total_Salary_Hospital = 0;
int Stor::count = 50;

int main()
{
	cout << "\n\n\t@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\n";
	cout << "\t@@ _______________________________________________________________ @@\n";
	cout << "\t@@|                                                               |@@\n";
	cout << "\t@@|                        WELCOME TO                             |@@\n";
	cout << "\t@@|                                                               |@@\n";
	cout << "\t@@|                 HOSPITAL MANAGEMENT SYSTEM                    |@@\n";
	cout << "\t@@|                                                               |@@\n";
	cout << "\t@@|          by:Hamdi & Omeed & Anas & Hajivan & Zakia            |@@\n";
	cout << "\t@@|                                                               |@@\n";
	cout << "\t@@|                                                               |@@\n";
	cout << "\t@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\n";


	int Choice;
	cout << "\n\t\t  HOSPITAL MANAGEMENT SYSTEM \n\n";
	cout << "\n\n  Please,  Choose from the following Options: \n";
	cout << "\t _________________________________________________________________ \n";
	cout << "\t|             1  >> Get all Information About Hospital            |\n";
	cout << "\t|             2  >> About Doctor                                  |\n";
	cout << "\t|             3  >> Queue And Info For Patient                    |\n";
	cout << "\t|             4  >> About Sister                                  |\n";
	cout << "\t|             5  >> About Employe                                 |\n";
	cout << "\t|             6  >> Pharmacy Room                                 |\n";
	cout << "\t|             7  >> inspect  Room                                 |\n";
	cout << "\t|             8  >> Stor  Hall                                    |\n";
	cout << "\t|             9  >> Park                                          |\n";
	cout << "\t|             10  >> Exit Hospital Management                     |\n";
	cout << "\t|_________________________________________________________________|\n\n";
	cout << "\t  Enter your choice: "; cin >> Choice;

	while (Choice != 10)
	{
		Hospital H("UOZ", "2019/27/11", "Kurdistan", 2, 6, 12, 8);
		if (Choice == 1) {
			H.Display_Hospital_Info();
		}
		if (Choice == 2)
		{
			int Ch;
			Doctor D(H, "Doctor", H.Set_Total_Doctor());
			cout << "\n\n\t  <-------------------->\n";
			cout << "\t  1-To Get The doctor with ID \n";
			cout << "\t  2- Print All info for Doctor \n";
			cout << "\t  3- Total Salary For Doctor in Hospital \n";
			cout << "\t  4- Exit About Doctor\n\t  <-------------------->";
			cout << "\n\t    Enter the choice: ";
			cin >> Ch;
			while (Ch != 4) {

				if (Ch == 1)
					Get_Doctor_ID(H, D);

				if (Ch == 2)
					D.Print_Info_Doctor(H);

				if (Ch == 3)
					D.Set_Total_Salaray_Doctor();

				cout << "\n\t    in { Doctor } Enter another choice: ";
				cin >> Ch;
			}
			false;
		}//end doctor if

		if (Choice == 3) {
			Patient P;
			P.Start();
		}//end patient 

		if (Choice == 4)
		{
			int Ch;
			Sister S(H, "Sister", H.Set_Total_Sister());
			cout << "\n\n<-------------------->\n";
			cout << "\t  1- To Get The Sister with ID \n";
			cout << "\t  2- Print All info for Sister \n";
			cout << "\t  3- Total Salary For Sister in Hospital \n";
			cout << "\t  4- Exit Management Sister\n<-------------------->\n";
			cout << "\t  Enter the choice: ";
			cin >> Ch;
			while (Ch != 4) {
				if (Ch == 1)
					Get_Sister_ID(H, S);
				if (Ch == 2)
					S.Print_Info_Sister(H);
				if (Ch == 3)
					S.Set_Total_Salaray_Sister();

				cout << "\n\t   in { Sister } Enter another choice: ";
				cin >> Ch;
			}
			false;
		}//end if sister

		if (Choice == 5)
		{
			int Ch;
			Employee E(H, "Employee", H.Set_Total_Employe());
			cout << "\n\n<-------------------->\n";
			cout << "\t  1- To Get The Employe with ID \n";
			cout << "\t  2- Print All info for Employe \n";
			cout << "\t  3- Total Salary For Employe in Hospital \n";
			cout << "\t  4- Exit management Employe\n<-------------------->\n";
			cout << "\t  Enter the choice: ";
			cin >> Ch;
			while (Ch != 4) {
				if (Ch == 1)
					Get_Employe_ID(H, E);//Friend
				if (Ch == 2)
					E.Print_Info_Employe(H);
				if (Ch == 3)
					E.Set_Total_Salaray_Employe();

				cout << "\n\t   in {Employee} Enter another choice: ";
				cin >> Ch;
			}
			false;
		}//end if employee

		if (Choice == 6)
		{
			int Ch;
			pharmacy ph;
			cout << "\n\n\t  <-------------------->\n";
			cout << "\t  1- Set medicament Name To get info about it\n";
			cout << "\t  2- Exit Pharmacy Room\n\t  <-------------------->\n";
			cout << "\t  Enter the choice: ";
			cin >> Ch;
			while (Ch != 2) {
				if (Ch == 1)
					ph.Set_Name();

				cout << "\n\t   in {Pharmacy} Enter another choice: ";
				cin >> Ch;
			}
			false;
		}//end if pharmacy

		if (Choice == 7)
		{
			int Ch;
			inspect I;
			cout << "\n\t  <-------------------->\n";
			cout << "\t  1- Set inspect kind To get info about it\n";
			cout << "\t  2- Exit inspect Room\n\t  <-------------------->\n";
			cout << "\t  Enter the choice: ";
			cin >> Ch;
			while (Ch != 2) {
				if (Ch == 1)
					I.set_Name();

				cout << "\n\t  in {inspect} Enter another choice: ";
				cin >> Ch;
			}
			false;
		}//end if pharmacy

		if (Choice == 8)
		{
			int Ch;
			Stor S;
			cout << "\n\t  <-------------------->\n";
			cout << "\t  1- Represent For Incpect in Stor\n";
			cout << "\t  2- Get All Place For Pharmacy\n";
			cout << "\t  3- Get khrgih after getting salary Hospital\n";
			cout << "\t  4- Exit inspect Room\n\t  <-------------------->\n";
			cout << "\t  Enter the choice: ";
			cin >> Ch;
			while (Ch != 4) {
				if (Ch == 1) {
					S.Represent();
				}
				if (Ch == 2)
					S.Stor_Pharmacy();
				if (Ch == 3)
					S.Get_Total_Mony_Hospital(H);

				cout << "\n\t  in {inspect} Enter another choice: ";
				cin >> Ch;
			}
			false;
		}//end if pharmacy
		if (Choice == 9)
		{
			int n;
			cout << "Enter car parking equal or less than " << C << " : ";
			cin >> n;
			while (n > C) {
				cin >> n;
			}
			park p;
			p.parkk(n);
			p.Print_car_park();
		}

		cout << "\n\t\t    >>>>> Main Choice  Enter another choise: ";
		cin >> Choice;
	}//end Main While 
	return 0;
}
